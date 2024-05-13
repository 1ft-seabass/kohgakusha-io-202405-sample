---
title: 2024 年 5 月執筆 GitHub
slug: 02-unity-setting
image_path: images/02-unity-setting/
---

![02-unity-setting_23.png](images/02-unity-setting/02-unity-setting_23.png)

Hierarchy を確認します。

## EventSystem の配置

![02-unity-setting_13.png](images/02-unity-setting/02-unity-setting_13.png)

Hierarchy で上部の＋ボタンから UI > EventSystem を選択します。

![02-unity-setting_24.png](images/02-unity-setting/02-unity-setting_24.png)

EventSystem が配置されます。

## MainCamera に PhysicsRaycaster コンポーネントの配置

![02-unity-setting_09.png](images/02-unity-setting/02-unity-setting_09.png)

MainCamera をクリックします。

![02-unity-setting_17.png](images/02-unity-setting/02-unity-setting_17.png)

MainCamera を選択した状態で Inspector を確認して Add Component をクリックします。

![02-unity-setting_01.png](images/02-unity-setting/02-unity-setting_01.png)

コンポーネントの選択画面が出てきます。

![02-unity-setting_11.png](images/02-unity-setting/02-unity-setting_11.png)

検索エリアに PhysicsRaycaster を検索してクリックして選択します。

![02-unity-setting_12.png](images/02-unity-setting/02-unity-setting_12.png)

PhysicsRaycaster が追加されました。

## Cube の配置

![02-unity-setting_05.png](images/02-unity-setting/02-unity-setting_05.png)

Hierarchy で上部の＋ボタンから 3D Object > Cude の操作をして Cube オブジェクトを配置します。

![02-unity-setting_08.png](images/02-unity-setting/02-unity-setting_08.png)

Cube というオブジェクトが作成されます。

![02-unity-setting_21.png](images/02-unity-setting/02-unity-setting_21.png)

Cube を選択した状態で Inspector を確認して Add Component をクリックします。

![02-unity-setting_10.png](images/02-unity-setting/02-unity-setting_10.png)

New script をクリックします。

![02-unity-setting_07.png](images/02-unity-setting/02-unity-setting_07.png)

Name が聞かれるので NodeRedPostOpenAI と入力して Create and Add をクリックします。

![02-unity-setting_14.png](images/02-unity-setting/02-unity-setting_14.png)

コンポーネントが追加されたら、C# スクリプトを編集するために、こちらをダブルクリックします。

![02-unity-setting_19.png](images/02-unity-setting/02-unity-setting_19.png)

エディタが起動します。NodeRedPostOpenAI のスクリプトは以下を記述して保存します。

[csharp]
using UnityEngine;
using UnityEngine.EventSystems;

using System.Collections;
using UnityEngine.Networking;
using System.Text;

using System;
using TMPro;

public class NodeRedPostOpenAI : MonoBehaviour, IPointerClickHandler
{
    // アクセスする URL
    string urlNodeRed = "ここにサーバーURLを入れる";

    // 送信する Unity データを JSON データ化する RequestData ベースクラス
    [Serializable]
    public class RequestData
    {
        public string message;
    }

    void Start()
    {
        Debug.Log($"Start");
    }

    public void OnPointerClick(PointerEventData eventData)
    {
        // マウスクリックイベント
        Debug.Log($"オブジェクト {this.name} がクリックされたよ！");

        // HTTP GET リクエストを非同期処理を待つためコルーチンとして呼び出す
        StartCoroutine(PostNodeRed());
    }

    // POST リクエストする本体
    IEnumerator PostNodeRed()
    {
        // HTTP リクエストする(POST メソッド) UnityWebRequest を呼び出し
        // アクセスする先は変数 urlNodeRed で設定
        UnityWebRequest request = new UnityWebRequest(urlNodeRed, "POST");

        // JSON データ作成
        RequestData requestData = new RequestData();
        // 今回は英語であいさつ
        requestData.message = "Hello!";
        // 送信データを JsonUtility.ToJson で JSON 文字列を作成
        // RequestData, RequestDataMessages の構造に基づいて変換してくれる
        string strJSON = JsonUtility.ToJson(requestData);
        Debug.Log($"strJSON : {strJSON}");
        // 送信データを Encoding.UTF8.GetBytes で byte データ化
        byte[] bodyRaw = Encoding.UTF8.GetBytes(strJSON);
        // アップロード（Unity→サーバ）のハンドラを作成
        request.uploadHandler = new UploadHandlerRaw(bodyRaw);
        // ダウンロード（サーバ→Unity）のハンドラを作成
        request.downloadHandler = new DownloadHandlerBuffer();
        // JSON で送ると HTTP ヘッダーで宣言する
        request.SetRequestHeader("Content-Type", "application/json");

        // リクエスト開始
        yield return request.SendWebRequest();

        // 結果によって分岐
        switch (request.result)
        {
            case UnityWebRequest.Result.InProgress:
                Debug.Log("リクエスト中");
                break;

            case UnityWebRequest.Result.Success:
                Debug.Log("リクエスト成功");

                // コンソールに表示
                Debug.Log($"responseData: {request.downloadHandler.text}");

                // テキストに反映
                GameObject.Find("Text1").GetComponent<TextMeshPro>().text = request.downloadHandler.text;

                break;
        }

        request.Dispose();
    }

}
[/csharp]

保存できたら以下のコードに注目します。

[csharp]
    // アクセスする URL
    string urlNodeRED = "ここにサーバーURLを入れる";
[/csharp]

「ここにサーバーURLを入れる」の部分を、今回の URL http://127.0.0.1:1880/api/openai に変更しておきます。

## TextMeshPro の配置・調整

![02-unity-setting_20.png](images/02-unity-setting/02-unity-setting_20.png)

Hierarchy で上部の＋ボタンから 3D Object > Text - TextMeshPro を選択します。

![02-unity-setting_16.png](images/02-unity-setting/02-unity-setting_16.png)

TMP Importer が表示されるので Import TMP Essentials をクリックします。

![02-unity-setting_02.png](images/02-unity-setting/02-unity-setting_02.png)

読み込まれたら閉じます。

![02-unity-setting_18.png](images/02-unity-setting/02-unity-setting_18.png)

Text (TMP) という名前で TextMeshPro が配置されました。

![02-unity-setting_06.png](images/02-unity-setting/02-unity-setting_06.png)

Text (TMP) をクリックすると名前が編集できるので Text1 と入力します。

![02-unity-setting_22.png](images/02-unity-setting/02-unity-setting_22.png)

名前が変更できました。

![02-unity-setting_00.png](images/02-unity-setting/02-unity-setting_00.png)

このままだと、テキストが大きすぎるのでちょっと調整します。

![02-unity-setting_03.png](images/02-unity-setting/02-unity-setting_03.png)

引き続き Text1 を選択した状態で Inspector を確認し Rect Transform に注目します。

![02-unity-setting_15.png](images/02-unity-setting/02-unity-setting_15.png)

- Pos X : 0
- Pos Y : 3
- Pos Z : 0
- Scale X : 0.5
- Scale Y : 0.5
- Scale Z : 0.5

と入力します。

![02-unity-setting_25.png](images/02-unity-setting/02-unity-setting_25.png)

また、Font Size を 12 に変更します。

![02-unity-setting_04.png](images/02-unity-setting/02-unity-setting_04.png)

テキストがこのように配置されます。

これで Unity の準備はできました。

## この記事の注意点

今回はテキストの表示は英語のみが表示される TextMeshPro のデフォルトで行っています。そのため、質問内容も Hello! という英語です。

もしも、質問内容を日本語で質問してテキストで日本語を表示したい場合は、以下の記事などを参考に TextMeshPro のフォントを日本語適用してから質問内容を日本語にして試してみてください。

- \[Unity\] Text Mesh Proで日本語を表示する方法
  - https://zenn.dev/kametani256/articles/63c083ab318136
