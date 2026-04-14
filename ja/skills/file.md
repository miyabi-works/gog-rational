# ファイル操作スキル

## Mono（物）
- 操作種別（operation）：string・必須
  read / write / move / copy / delete / list のいずれか
- 対象パス（path）：string・必須・空文字不可
- 移動先パス（destination）：string・moveとcopy時のみ必須
- 内容（content）：string・write時のみ必須
- 上書き許可（overwrite）：boolean・任意・デフォルトfalse

## Koto（事）
照合① → 照合② → 照合③ → ファイル操作実行

## Imi（意・照合条件）

### 照合①（開始時）
- 対象パスが存在するか（readの場合）
- 操作種別が許可リストのいずれかか
- 実行権限があるか

### 照合②（実行中）
- pathが許可フォルダ内か
  （システムフォルダ・.env・APIキー含むファイルは禁止）
- deleteの場合：対象が存在するか
- writeの場合：overwrite=falseで既存ファイルがある場合は中断
- moveの場合：destinationが許可フォルダ内か
- 再帰的削除（rm -rf相当）は無条件禁止

### 照合③（確定時）
- 照合①②が全て適合した場合のみ実行可能に遷移
- delete・move・overwriteは必ず実行前に確認を求める
- 不可逆操作（delete）は特に慎重に確認する
- 1件でも不適合なら実行しない・理由を報告する

## 禁止事項
- 照合③未通過での実行
- システムフォルダへのアクセス
- .env・APIキー・認証情報ファイルへのアクセス
- 再帰的削除
- 確認なしのdelete・move・overwrite
