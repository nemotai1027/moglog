# moglog

## Firebaseの設定

募集データはCloud Firestoreの`recruitments`コレクションに保存します。新しい投稿はFirestoreのリアルタイム購読で、開いている全員のページに反映されます。

1. Firebase Consoleでプロジェクトを作成する。
2. Webアプリを追加し、表示された設定値を`firebase-config.js`にコピーする。
3. Firestore Databaseを作成する。
4. 開発確認用として、Firestoreのルールを次のように設定する。

```text
rules_version = '2';
service cloud.firestore {
	match /databases/{database}/documents {
		match /recruitments/{recruitmentId} {
			allow read, create: if true;
			allow update, delete: if false;
		}
	}
}
```

このルールは誰でも投稿できる設定です。公開運用ではFirebase Authenticationを導入し、認証済みユーザーだけが投稿できるルールに変更してください。

GitHub PagesなどのWebサーバー経由で`index.html`を開いてください。`file://`で直接開くと、Firebase SDKやブラウザの制限で正常に動作しない場合があります。