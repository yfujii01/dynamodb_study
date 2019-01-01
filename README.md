# dynamodb_study

## dynamodb-localをインストール

ダウンロードして解凍

https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/DynamoDBLocal.html


## 起動

```
$ java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
```

## ブラウザを使って操作

http://localhost:8000/shell/

## テーブル作成

```
AWS.config.endpoint = new AWS.Endpoint('http://localhost:8000');
var dynamodb = new AWS.DynamoDB();

// テーブル作成
// https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/SQLtoNoSQL.CreateTable.html

var params = {
    TableName : "Music",
    KeySchema: [       
        { 
            AttributeName: "Artist", 
            KeyType: "HASH", //Partition key
        },
        { 
            AttributeName: "SongTitle", 
            KeyType: "RANGE" //Sort key
        }
    ],
    AttributeDefinitions: [
        { 
            AttributeName: "Artist", 
            AttributeType: "S" 
        },
        { 
            AttributeName: "SongTitle", 
            AttributeType: "S" 
        }
    ],
    ProvisionedThroughput: {
        ReadCapacityUnits: 1, 
        WriteCapacityUnits: 1
    }
};
dynamodb.createTable(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

## テーブル削除

```
// テーブル削除
// https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/SQLtoNoSQL.RemoveTable.html

var params = {
    TableName: 'Music',
};
dynamodb.deleteTable(params, function(err, data) {
    if (err) ppJson(err);
    else ppJson(data);
});
```

## データ追加

```
AWS.config.endpoint = new AWS.Endpoint('http://localhost:8000');
var dynamodb = new AWS.DynamoDB();

// データ追加
// https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/SQLtoNoSQL.WriteData.html

var params = {
    TableName: "Music",
    Item: {
        "Artist":{S:"No One You Know"},
        "SongTitle":{S:"Call Me Today"},
        "AlbumTitle":{S:"Somewhat Famous"},
        "Year": {N:"2015"},
        "Price": {N:"2.14"},
        "Genre": {S:"Country"},
        "Tags": {
            L : [
                {S:"AAA"},
                {S:"BBB"}
                ]
        },
    }
};
dynamodb.putItem(params, function(err, data) {
    if (err) ppJson(err);
    else ppJson(data);
});
```

## テーブル一覧の取得

```
dynamodb.listTables().eachPage(function(err, data) {
    if (err) {
        ppJson(err); // an error occurred
    } else if (data) {
        ppJson(data);
    }
});
```


## アイテム取得

```
var params = {
    TableName: 'Persons',
    Key: { 
        Id: {N:'5'}
    }
};
dynamodb.getItem(params, function(err, data) {
    if (err) ppJson(err);
    else ppJson(data);
});
```
