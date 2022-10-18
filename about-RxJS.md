# RxJS

## ■ RxJSとは
リアクティブプログラミングを行うJavaScriptライブラリ

```
▶︎ リアクティブプログラミング
流れてくるストリーム(データ)に対して、関連性と操作を”宣言”的に記述するプログラミングの手法。

(ex)
- 検索処理による検索キーワード
- HttpClient のリクエスト

▶︎ Angularでは、非同期処理で流れてくるデータを扱うので、RxJSが効果的に機能する(複雑になりがちなものもスッキリ扱える)。
```

### ◎ Observable と Subscribe
Observable で川を作り、上流から取得したデータを流す ← この時点ではまだ川(ストリーム)の流れはない</br>
Observable の Subscribe メソッドで、川の流れを発生させてデータを受け取る</br>
※ Observable だけでは川の流れは発生しない。

```
▶︎ 例えるなら1本の川
① 何かしらの操作に伴って、任意のデータを川(ストリーム)に流す
- ユーザーのマウス操作
- キーボード操作
- HttpClient でデータ取得

② 中間処理(オペレータ)
pipeメソッドの中で指定した関数(map, filter ...等)
(ex)
- 途中で拾って加工したり、入れ替えたりする
- 条件に合うものだけをフィルタリングする

③ 下流でキャッチする
subscribe　メソッドでデータを受け取る
（= オブジェクト間のデータのやり取りを行う）
```
(ex)
```
delete(member: Member): void {
  this.members = this.members.filter(m => m!== member)
  this.memberService.deleteMember(member).subscribe() // HttpClientのdeleteメソッドを実行するために川の流れを発生させている
```

```
Observable.subscribe(
  value => console.log(value),            // 第一引数 データを受け取った際のコールバック
  err   => {},                            // 第二引数　エラー時のコールバック
  ()    => console.log('this is the end') // 第三引数 処理完了時のコールバック
```

### ◎ async パイプ
Angular には、テンプレート(HTML)で展開する仕組みがある
```
subscribe を実行することなく、テンプレート側で自動的にデータを受け取る
単純に、「流れてくるデータを表示するだけ」という時におすすめ
```

### ◎ オペレータ(中間処理)について
- map()</br>
ストリームで流れてきたデータを受け取り、違うデータを返す。
```
Observable
  .pipe(
    map(value => `map: ${value}`)
  )
```

- filter()</br>
データ検証の目的で使用。</br>
流れてきたデータに対してチェックを行い、条件に合えば、後のストリームにデータを流す。
```
Observable
  .pipe(
    filter(value => value === 'test',
    map(value => `filter() matched: ${value}`)
  )
```

- debounceTime()</br>
連続してデータがストリームに流れた場合、最後の処理から指定した秒数だけ遅延した後に、一度だけデータをストリームに流す。
```
Observable
  .pipe(
    debounceTime(250),
    map(value => `debounceTime(250): ${value}`)
  )
```

- skip()</br>
指定した回数分だけストリームを流さずにスキップする。
```
Observable
  .pipe(
    skip(3),  // 3回スキップされた後、4回目でデータが流れる。
    map(value => `skip: ${value}`)
  )
```

- take()</br>
指定した回数分だけ実行(それ以降、ストリームにデータが流れない)
```
Observable
  .pipe(
    take(3),  // 3回実行され、4回目以降はデータが流れない。
    map(value => `take({++ count}): ${value}`)
  )
```
