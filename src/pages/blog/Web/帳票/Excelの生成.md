---
title: "Excelの生成"
templateKey: blog-post
date: 2020-07-09T16:54:00+09:00
tags: [Python, Excel]
draft: true
---

## Excel の生成

- pandas \- Python Data Analysis Library Python のデータ処理ライブラリ。
- XlsxWriter Python
- openpyxl Python
- ExCella Reports java。オープンソース。Excel と PDF の両方の出力に対応(次のセクションも参照)

See also: [サーバサイドで Excel ブックを生成するいくつかの方法 \- Qiita](https://qiita.com/matarillo/items/549493f6bb7e36c99443)

### openpyxl

#### 動作確認環境

以下の環境で動作確認しました。

- macOS 10.15.5
- Python 3.8.4
- openpyxl 3.0.4
- npm 6.3.4 (node 10.19.0)

Python3 はあらかじめインストール済みであると仮定します。

#### 手順

```bash
pip install openpyxl
```

See Also: [Tutorial — openpyxl 3\.0\.5 documentation](https://openpyxl.readthedocs.io/en/latest/tutorial.html)

```python
# coding: utf-8

import openpyxl
import subprocess as sub


class ReportExcel:
    """Excel帳票管理
    """

    # SCORE_SHEET_SHEET = 'ScoreSheet'
    VALUES_SHEET = 'Values'

    def __init__(
        self,
        sourceExcelFile="../Template/BD-Score.xlsx",
        destPath='../Output/',
        targetExcelFile='excel-output.xlsx',
    ):
        """コンストラクタ
        """
        self.sourceExcelFile = sourceExcelFile
        self.destPath = destPath
        self.targetExcelFile = targetExcelFile

    def generateExcel(self, valueMap):
        """Excelファイルの生成
        """
        wb = openpyxl.load_workbook(self.sourceExcelFile)
        # ws = wb[self.SCORE_SHEET_SHEET]
        ws2 = wb[self.VALUES_SHEET]
        # Excelファイルの書き換え
        isError = False
        errorKeys = []
        for teamKey, teamValue in valueMap.items():
            for cellKeyBase, cellValue in teamValue.items():
                cellKey = teamKey + '_' + cellKeyBase
                try:
                    self.__setValueToDefinedName(wb, cellKey, cellValue)
                except KeyError:
                    errorKeys.append(cellKey)
                    isError = True
        # 名前エラーをまとめて一つのエラーとして発生
        if isError:
            messages = "KeyError: {keys}".format(keys=', '.join(errorKeys))
            print(messages)
            raise Exception(messages)
        # Valuesシートの非表示（印刷対象外にするため）
        ws2.sheet_state = 'hidden'
        # Excelファイルの保存
        targetExcelFillPath = self.destPath + self.targetExcelFile
        wb.save(targetExcelFillPath)


    def __setValueToDefinedName(self, wb, name, value):
        """Excelの名前付きセルに値を設定する

        Args:
            wb(Workbook)
            name(string): 名前付きセル
            value(string or int): 設定する値
        Raises:
            KeyError
        """
        range = wb.defined_names[name]
        dests = range.destinations
        cells = []
        for title, coord in dests:
            ws = wb[title]
            cells.append(ws[coord])
            # print('coord: ', coord)
        if len(cells):
            oneCell = cells[0]
            oneCell.value = value


if __name__ == "__main__":
    valueMap = {
        'A': {
            "TeamName": "日本",
            "Player1": "織田信長",
            "Player2": "豊臣秀吉",
            "LR": "L",
            "Point1": 21,
            "Point2": 0,
            "Point3": 30,
            "Game": 2,
        },
        'B': {
            "TeamName": "中国",
            "Player1": "夏煊泽",
            "Player2": "蔡赟",
            "LR": "R",
            "Point1": 19,
            "Point2": 21,
            "Point3": 28,
            "Game": 1,
        }
    }

    reportExcel = ReportExcel(
        sourceExcelFile="../Template/BD-Score.xlsx",
        destPath='../Output/',
        targetExcelFile='excel-output.xlsx',
    )
    reportExcel.generateExcel(
        valueMap
    )
```

###
