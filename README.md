# Javaコード&&JVM設定メモ
よく使うJavaチューニングオプション
## -XX options
`-XX:+PrintAssembly` ... Javaの実行時にアセンブル出力(ddlの設置が必要)
`-XX:+UnlockDiagnosticVMOptions`も必要
`-XX:UseAVX` ... CPU命令セットを変更(使えるレジスタも変更)
`-XX:CompilerDirectivesFile=` ... Compiler Directoryを指定してJITコンパイルの挙動を変更
`-XX:+UnlockDiagnosticVMOptions`必要
`-XX:-TieredCompilation` ... C2コンパイラのみ使う

## -Xlog options
`-Xlog:os=info` ... 基本、ULログでos,gcなどの見たいログ情報レベル別で狙い撃ち
`-Xlog:jit+compilation=debug` ... ULログでJITコンパイルの最適化レベルを見る

## その他コマンド
`-Xms` ... ヒープ・メモリ全体の起動時のサイズ  
`-Xmx` ... ヒープ・メモリ全体の最大サイズ(New + Old)  
`-Xmn` ... NEW領域のサイズ  
`-XX:SurvivorRatio=` ... Eden領域のサイズをFromまたはTo領域のサイズで割った値（FromとTo領域は同じサイズ）  
`-verbose:gc` ... ガベージ・コレクション（GC）を詳細にトレースする  

プロファイリング
JFR

Async Profiler