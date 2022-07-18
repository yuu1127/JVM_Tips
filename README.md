# 役に立つJavaチューニングオプション
## -XX options
`-XX:+PrintAssembly` ... Javaの実行時にアセンブル出力(ddlの設置が必要)  
`-XX:+PrintFlagsFinal` ... option確認(どの命令でc1,c2使ったか見るのにも便利)  
`-XX:CompileCommand=print,<Class>::<Method>` ... アセンブラ出力メソッド狙い撃ち  
`-XX:+UnlockDiagnosticVMOptions`も必要  
`-XX:UseAVX=<Version>` ... CPU命令セットを変更(使えるレジスタも変更)(x86 only)  
`-XX:CompilerDirectivesFile=` ... Compiler Directoryを指定してJITコンパイルの挙動を変更  
`-XX:-TieredCompilation` ... C2コンパイラのみ使う  
`-XX:StartFlightRecording=settings=profile,filename=<Sample.jfr>` ... JFRを使用  
`-XX:+UseParallelGC` `-XX:+UseParallelGC` ... GCのアルゴリズムの変更  
`-XX:+PrintGCDetails` ... GCの詳細情報表示

## -Xlog options
`-Xlog:os=info` ... 基本、ULログでos,gcなどの見たいログ情報レベル別で狙い撃ち。trace,debug,info,warning,error
`-Xlog:gc+heap` ... でもgc+heapの情報を出力
`-Xlog:jit+compilation=debug` ... ULログでJITコンパイルの最適化レベル(tier1~4を見る)
`-Xlog:gc:file=gc.log` ...  fileにgcの内容を出力。左から順にJVM起動からの経過時間、ログレベル、タグ、メッセージ


## その他Java関連コマンド
`jcmd <PID> help VM.log` ... Javaプロセスの設定を調べる
jcmd $(pidof java) help VM.logだと使いやすい
`jcmd <PID> VM.log output="sample.log"` ... ファイルに出力
`-Xms` ... ヒープ・メモリ全体の起動時のサイズ  
`-Xmx` ... ヒープ・メモリ全体の最大サイズ(New + Old)  
`-Xmn` ... NEW領域のサイズ  
`-XX:SurvivorRatio=` ... Eden領域のサイズをFromまたはTo領域のサイズで割った値（FromとTo領域は同じサイズ）  
`-verbose:gc` ... ガベージ・コレクション（GC）を詳細にトレースする  
`jdeps <sample.jar>` ... 依存しているパッケージを表示)module-info.javaなどを作成するときに使う
maven clean install で意図したJarが入っているかなど

## その他おまけ
Async Profiler ... どのメソッドがボトルネックかをGUI(html形式)で表示    
maven ... `maven clean install`などのコマンドでJavaアプリケーションを作成するのに便利、依存するライブラリを自動でインストールしてくれる ~/.m2mavenにそのライブラリは保存されている    
shade jar ... ライブラリなどに入っているクラスの全てjar以下のディレクトリに置いたいわば解凍されたJar、LambdaなどのFaaSだとこの形態じゃないと
classを認識してくれない  
thin jar ... mavenでライブラリ(jdbcなど)が全て入ったJar、これを持っていけばどこでも動く。
逆によく見るoriginal Jarはその外部ライブラリが入っていない素のJar  
jarコマンド ... jarの作成、展開、中身などを確認できるコマンド  

## Javaでなぜつくるのかより
- JavaはCと違いポインタではなくポインタへの参照を使っているのでメモリに直接アクセスできない
- Cと違い、Javaはオブジェクトしかヒープに置けない（配列もオブジェクト扱い）
配列の参照型変数はスタックに置かれる、そのヒープの中の要素であるStringなどはプログラム領域に置かれる
（プログラム領域に置かれるのはリテラル値、コンスタント、GCの対象にはならない ）
- Index Out of Boundsはオブジェクトのメタデータに長さが書かれていて、実行時に検査される。
- GCは参照されていないオブジェクトを探してfree()する。
- Javaはパッケージ外だと同じ名前のクラスがあっていい
- JavaはC,C++と違いリンクをまとめた実行ファイルをつくらず、代わりのクラスローダーが実行直後に行われる（だから少し遅い）