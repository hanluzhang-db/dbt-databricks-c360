# dbt on Databricks demo
---

This content demo how Databricks can run dbt pipelines, integrated with Databricks Workflow.

This demo replicate the Delta Live Table (DLT) pipeline in the lakehouse c360 databricks demo available in `dbdemos.install('lakehouse-retail-c360')`

## Running dbt on Databricks


This demo is part of [dbdemos.ai](http://www.dbdemos.ai) dbt bundle. <br/> Do not clone this repo directly. 

Instead, to install the full demo with the worfklow and repo, you can run:

```
%pip install dbdemos
dbdemos.install('dbt-on-databricks')
```

The best way to run production-grade dbt pipeline on Databricks is as a [Databricks Workflow dbt Task](https://docs.databricks.com/workflows/jobs/how-to-use-dbt-in-workflows.html).

Here is an overiew of the workflow created by dbdemos:

<img width="800px" src="https://raw.githubusercontent.com/databricks-demos/dbdemos-resources/main/images/partners/dbt/dbt-databricks-workflow.png" /><br/>
Task 02 is a dbt task running on Databricks workflow directly.


## Running dbt + databricks locally

```
pip install dbt-databricks
export DBT_DATABRICKS_HOST=xxxx.cloud.databricks.com  
export DBT_DATABRICKS_HTTP_PATH=/sql/1.0/warehouses/xxx 
export DBT_DATABRICKS_TOKEN=dapixxxxx 
dbt run
```

### Project structure



This demo is broken up into the following building blocks. View the sub-folders in in the sequence indicated below to help you understand the overall flow:


- ```01-ingest-autoloader``` <br/>
    * このノートブックは、レイクハウスに生データを段階的に取り込むためのものです（dbtのタスクではありません）
    * 目的は、クラウドストレージに新しいデータがアップロードされたら、そのデータを取り込むことです。これにより、dbtパイプラインで変換処理が行えます
    * dbtにはファイルをロードする機能「seed」がありますが、現在は```CSV```ファイルに限定されている点に注意が必要です

- ```dbt_project.yml```
    * すべてのdbtプロジェクトには```dbt_project.yml```ファイルが必要です。これにより、ディレクトリがdbtプロジェクトであることをdbtが認識します
    * このファイルには、Databricks SQLウェアハウスへの接続設定やSQL変換ファイルの保存場所などの情報が含まれています

- ```profiles.yml```
    * このファイルは、dbtがDatabricksの計算リソースに接続するために必要なプロファイル設定を保存しています
    * サーバーホスト名、HTTPパス、カタログ、データベース/スキーマ情報などの接続詳細がここで設定されます

- ```models```
    * dbtでのモデルは、モジュール化されたデータ変換ブロックを含む単一の```.sql```ファイルを指します
    * このデモでは、メダリオンアーキテクチャに従って変換を4つのファイルにモジュール化しています
    * 各ファイル内で、変換がテーブルとして実現されるか、ビューとして実現されるかを設定できます

- ```tests```
    * テストは、dbtモデルに関して行うアサーション（主張、断言）です
    * 通常、データ品質と検証目的で使用されます
    * 特定のアサーションに失敗したレコードを隔離し、孤立させる能力も持っています

- ```03-ml-predict-churn```
   * このノートブックは、dbt変換が完了した後にMLFlowから顧客流失予測MLモデルをロードするためのものです（dbtのタスクではありません）
   * モデルはSQL関数としてロードされ、ワークフローの第二dbtタスクの終わりに具現化される```dbt_c360_gold_churn_features```に適用されます

- ```seeds```
    * これは、レイクハウスにロードされるサンプルやアドホックなCSVファイルを保存するために使用されるオプショナルなフォルダです。デフォルトの設定では使用されません（代わりにオートローダーでの摂取を使用しています）


<br>

<img src="https://mchanstorage2.blob.core.windows.net/mchan-images/databricksDbtHeader.png" width="525px" />

<img width="1px" src="https://www.google-analytics.com/collect?v=1&gtm=GTM-NKQ8TT7&tid=UA-163989034-1&cid=555&aip=1&t=event&ec=field_demos&ea=display&dp=%2F42_field_demos%2Ffeatures%2Fdbt%2Freadme&dt=FEATURE_DBT" />



### Feedback
---
Got comments and feedback? <br/>
Feel free to reach out to ```mendelsohn.chan@databricks.com``` or ```quentin.ambard.databricks.com```









