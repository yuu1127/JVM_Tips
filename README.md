# Javaコード&&JVM設定メモ
よく使うJavaチューニングオプション
## -XX options
`-XX:+PrintAssembly` ... Javaの実行時にアセンブル出力(ddlの設置が必要)  
`-XX:+PrintFlagsFinal` ... option確認(どの命令でc1,c2使ったか見るのにも便利)  
`-XX:CompileCommand=print,<Class>::<Method>` ... アセンブラ出力メソッド狙い撃ち
`-XX:+UnlockDiagnosticVMOptions`も必要  
`-XX:UseAVX=<Version>` ... CPU命令セットを変更(使えるレジスタも変更)(x86 only)  
`-XX:CompilerDirectivesFile=` ... Compiler Directoryを指定してJITコンパイルの挙動を変更  
`-XX:+UnlockDiagnosticVMOptions`必要
`-XX:-TieredCompilation` ... C2コンパイラのみ使う  
`-XX:StartFlightRecording=settings=profile,filename=<Sample.jfr>` ... JFRを使用  
`-XX:+UseParallelGC` `-XX:+UseParallelGC` ... GCのアルゴリズムの変更  
`-XX:+PrintGCDetails` ... GCの詳細情報表示

## -Xlog options
`-Xlog:os=info` ... 基本、ULログでos,gcなどの見たいログ情報レベル別で狙い撃ち  
`-Xlog:jit+compilation=debug` ... ULログでJITコンパイルの最適化レベル(tier1~4を見る)

## その他コマンド
`jcmd <PID> help VM.log` ... Javaアプリケーションの設定を調べる
jcmd $(pidof java) help VM.logだと使いやすい
`-Xms` ... ヒープ・メモリ全体の起動時のサイズ  
`-Xmx` ... ヒープ・メモリ全体の最大サイズ(New + Old)  
`-Xmn` ... NEW領域のサイズ  
`-XX:SurvivorRatio=` ... Eden領域のサイズをFromまたはTo領域のサイズで割った値（FromとTo領域は同じサイズ）  
`-verbose:gc` ... ガベージ・コレクション（GC）を詳細にトレースする  

## その他おまけ
Async Profiler ... なんのメソッドがボトルネックかGUIで表示、便利