Web クローラー
========

クローラーとは
--------
統計調査などを行う際に、特定のWebリソースにアクセスしデータを抽出する一連の作業をプログラムで作成することがあります。
このようなプログラムを「Webクローラー」「Webスクレイパー」「Webスパイダー」などと呼びます。

### 注意
APIなどが提供されている場合はそれを利用するようにしてください。


利用例
========
ここではPythonを利用したクローラーの例を紹介します。
`beautifulsoup4` というライブラリを使用します。

対象は[e-Statの労働力調査](http://www.e-stat.go.jp/SG1/estat/GL08020102.do?_toGL08020102_&tclassID=000000110001&cycleCode=1&requestSender=dsearch)で、
毎年月の「産業，従業上の地位・雇用形態（雇用者については従業者規模）別就業者数」のExcelファイルを取得していきます。

プログラムの流れは以下です。

1. 対象のページをHTTP GETする。すると、レスポンスとしてHTML文書が返ってくる。
2. HTML文書をBeautifulSoupに通し、HTMLエレメントの階層的なオブジェクトに変換する。
3. できたsoupオブジェクトを利用し、目的のコンテンツのURIまで到達する。
	1. まずは各月の「統計表一覧」ページに到達するリンクのURIを取得し、そのページを取得する。
	2. 取得したページをsoup化して、ExcelのURIを取得する。
	3. ExcelのURIをHTTP GETし、自分のファイルに保存する。
	4. これを、全ての月について行う。

目的のリソースに到達するためのHTML構造を調べるために、HTML文書を読む他にブラウザの「要素を検証」機能などが役に立ちます。

```python
import re
import urllib.request
import urllib.parse
from bs4 import BeautifulSoup as bs


number = re.compile('\d+')


def get_rows(soup):
    """一覧のページから年ごとのtrエレメントを取得します"""
    for row in soup.table.find_all('tr'):
        raw_year = row.td.strong.string
        year = number.search(raw_year).group()
        yield year, row


def get_monthly_page(row):
    """一覧のページから月ごとのURLを取得します"""
    for anchor in row.find_all('a'):
        raw_month = anchor.string
        month = number.search(raw_month).group()
        yield month, anchor['href']


def fetch_excel(url):
    """\
    各月のページから
    「産業，従業上の地位・雇用形態（雇用者については従業者規模）別就業者数」
    のエクセルファイルを取得します。
    """
    src = urllib.request.urlopen(url)
    soup = bs(src, "html.parser")
    # HTMLを読み、目的のデータをもったhtmlエレメントに到達します
    row = soup.find_all('tr')[11]
    data = row.find_all('td')[2]
    # aエレメントが示すURIを取得します
    uri = urllib.parse.urljoin(url, data.a['href'])
    # 取得したURIからファイルを取得します
    return urllib.request.urlopen(uri)


def save_excel(response, year, month):
    # yyyy_mm.xlsという名前で保存します。
    fname = '_'.join([year, month]) + '.xls'
    with open(fname, 'wb') as f:
        f.write(response.read())


if __name__ == '__main__':
    URL = "http://www.e-stat.go.jp/SG1/estat/GL08020102.do" \
        "?_toGL08020102_&tclassID=000000110001&cycleCode=1"
    src = urllib.request.urlopen(URL)
    soup = bs(src, "html.parser")

    for year, row in get_rows(soup):
        for month, url in get_monthly_page(row):
            uri = urllib.parse.urljoin(URL, url)
            res = fetch_excel(uri)
            save_excel(res, year, month)
```

Robotstxt
========
クローラーのように自動的にWebコンテンツにアクセスするプロセスのことを「ロボット」「ボット」といいます。
サービスの品質や法的な理由から、ウェブマスターはロボットがアクセスしていいページ、アクセスしないでほしいページを指定している場合があります。
これをRobotstxtといいます。

Robotstxtは階層的に指定されており、`../robots.txt`というURIで配信されています。
Robotstxtは各階層に分散して配置可能で、各Robotstxtは自分以下の階層に対して効果を持っており、深い階層に上書きされます。
つまり、`http://example.com/experiments/for/Robotstxt`というページをロボットでアクセスしたい場合、

- `http://example.com/robots.txt`
- `http://example.com/experiments/robots.txt`
- `http://example.com/experiments/for/robots.txt`

を読み、ロボットのアクセスが許されているかどうかチェックするべきです。
`robots.txt`が全く存在しない場合は、まあ、うん。

Robotstxtには他にもサイトマップを指定する機能があり、これを利用してロボットは対象のWebサイトのコンテンツを適切に網羅することができます。

Robotstxtについて詳細は以下を参照してください。

http://www.robotstxt.org/
