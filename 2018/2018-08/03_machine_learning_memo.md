# 機械学習メモ

昨日縁があって調べたことを備忘録として.

## 精度について

1. 学習済の DNN を使って推論を実行する場合は INT8(or INT4) で OK
2. DNN の学習処理は FP16(or bfloat16) が必要

## FP16 と bfloat16 の違い

FP16 は IEEE754 の半精度浮動小数点で標準規格だが、Google TPU v2 はそれとは違う bfloat16 をサポートしている. Intel も Cooper Lake で対応するようなので、bfloat16 が事実上の標準になるのかもしれない.

https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/bfloat16.h
> We don't use that format here because conversion to/from 32-bit floats is more complex for that format, and the conversion for this format is very simple.

本家を見るに、bfloat16 は変換が楽、それに尽きるよう.
FP32 の前 16bit を取る(= 後ろ16bit を捨てる)だけで bfloat16 に変換できるのでそりゃそうだではあるが.
NaN でない FP32 は必ず bfloat16 に変換できる(当然精度は落ちる)が、FP32 から FP16 への変換は必ずしも可能ではないことにも注意しておきたい.

## データ形式

NCHW とか NHWC とか表記されるデータ形式が存在する.
それぞれのアルファベットの意味は以下.

https://www.tensorflow.org/performance/performance_guide#data_formats

> * N refers to the number of images in a batch.
> * H refers to the number of pixels in the vertical (height) dimension.
> * W refers to the number of pixels in the horizontal (width) dimension.
> * C refers to the channels. For example, 1 for black and white or grayscale and 3 for RGB.
