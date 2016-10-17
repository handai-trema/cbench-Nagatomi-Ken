##Cbenchのボトルネック調査
RubyのプラファイラでCbenchのボトルネックを解析しよう。

##解答
まず、端末で

```
ruby -r profile ./bin/trema run ./lib/cbench.rb &> kekka_prolile.txt
```
と入力しtremaを実行した後、別の端末で


switch_disconnectedを用いて、切断時に切断されたスイッチを表示するようにした。

別の端末で
```
./bin/cbench --port 6653 --switches 1 --loops 10 --ms-per-test 10000 --delay 1000 --throughput
```
を実行し、プログラムが終了した後にまた最初の端末でtremaを終了させた。

出力されたkekka_profile.txtの結果の一部を以下に示す
```
Cbench started.
  %   cumulative   self              self     total
 time   seconds   seconds    calls  ms/call  ms/call  name
187.12   185.40    185.40        2 92700.00 92700.00  IO.select
 93.56   278.10     92.70        2 46350.00 46350.00  Thread#join
 93.56   370.80     92.70        2 46350.00 46350.00  TCPServer#accept
 93.56   463.50     92.70      114   813.16   813.16  Kernel#sleep
  3.24   466.71      3.21    49409     0.06     0.08  Kernel#dup
  3.24   469.92      3.21    57169     0.06     0.31  BinData::Struct#instantiate_obj_at
  2.22   472.12      2.20    83556     0.03     0.03  Kernel#define_singleton_method
  2.19   474.29      2.17    66525     0.03     0.10  BinData::BasePrimitive#method_missing
  2.04   476.31      2.02   146981     0.01     0.06  BasicObject#!=
```
この結果より、IQ.selectが最も時間がかかっており、ボトルネックになっていると考えられる。
