---
title: "Chapter 8장"
author: "Hanjeongin"
date: 2022-04-11 23:50:00
tags: Python
---

출처 : 혼자 공부하는 머신러닝+딥러닝

# 순차 데이터와 순환 신경망

- 초급 레벨 : 기초통계 (t.test, 분산분석, 회귀분석 등)
- 중급 레벨 : 시계열 분석 / 베이지안 / 비모수검정
- 시계열 데이터 : 주식 / 날씨 / 매장 매출
  + R로 공부를 해야함

# 텍스트

- 텍스트 마이닝 (데이터 분석가)
  + 대표적으로 감정분석 (긍정 / 부정 분류)
  + 문자열 : 인코딩하는 방법론이 존재
- 자연어 처리 (개발자에 해당)
  + 챗 봇 (툴은 다 존재함)
  + 자동 번역

- 기본 딥러닝 알고리즘 / RNN & LSTM
  + 현실에선 안씀

- 자료
  + 딥러닝을 이용한 자연어 처리 입문 (텐서플로) : https://wikidocs.net/book/2155
  + Pytorch로 시작하는 딥러닝 입문 : https://wikidocs.net/32471

- 분야 선정
  + 영상인식, 이미지 분류, 음성, 자연어

# 순환 신경망

- 이미지는 픽셀값이 어느정도 고정되어 있음
  + 28 x 28로 정의 / 모든 데이터는 28 x 28 맞출 수 있음

  - 텍스트
    + 값이 고정이 불가함

- 교재 494페이지
- i am a boy(1, 4, 3)
- I am a hansome boy(1, 4, 1, 2

# 순환 신경망 IMDB 리뷰 분류

- 주제 : 긍정리뷰 부정리뷰 분류
- 501p
  + 텍스트 자체가 신경망에 전달하지 않는다! (문자열 -> 수식에 적용 X)
  + 문자열을 수식으로 정하는 규칙이 매우 가변적임 (토큰화, Tokenizing)
  + He follows the cat. He loves the cat.
  + 10 11      12  13   10 14    12  13
  + 고양이를 따라간다. He follows the cat.
  + 10       11        12 13      14  15

- RNN, LSTM 알고리즘
  + 영어권 사람들이 만듦
  + 자연어처리와 관련된 많은 알고리즘
  + 영어권 사람이 만듦

- 한글 != 영어
  + 성과를 내려면 제품을 써야함 (= 돈) (네이버)

## 데이터 불러오기


```python
from tensorflow.keras.datasets import imdb
(train_input, train_target), (test_input, test_target) = imdb.load_data(num_words = 500)
```

- 데이터 크기 확인 (1차원 배열)


```python
print(train_input.shape, test_input.shape)
```

    (25000,) (25000,)
    

- 문장의 길이가 다 다름


```python
print(len(train_input[0]))
print(len(train_input[1]))
print(len(train_input[2]))
```

    218
    189
    141
    

- Raw 데이터 전처리 -> 토큰화 작업이 끝난 상황 (문자열 -> 숫자로 바뀜)


```python
print(train_input[0])
```

    [1, 14, 22, 16, 43, 2, 2, 2, 2, 65, 458, 2, 66, 2, 4, 173, 36, 256, 5, 25, 100, 43, 2, 112, 50, 2, 2, 9, 35, 480, 284, 5, 150, 4, 172, 112, 167, 2, 336, 385, 39, 4, 172, 2, 2, 17, 2, 38, 13, 447, 4, 192, 50, 16, 6, 147, 2, 19, 14, 22, 4, 2, 2, 469, 4, 22, 71, 87, 12, 16, 43, 2, 38, 76, 15, 13, 2, 4, 22, 17, 2, 17, 12, 16, 2, 18, 2, 5, 62, 386, 12, 8, 316, 8, 106, 5, 4, 2, 2, 16, 480, 66, 2, 33, 4, 130, 12, 16, 38, 2, 5, 25, 124, 51, 36, 135, 48, 25, 2, 33, 6, 22, 12, 215, 28, 77, 52, 5, 14, 407, 16, 82, 2, 8, 4, 107, 117, 2, 15, 256, 4, 2, 7, 2, 5, 2, 36, 71, 43, 2, 476, 26, 400, 317, 46, 7, 4, 2, 2, 13, 104, 88, 4, 381, 15, 297, 98, 32, 2, 56, 26, 141, 6, 194, 2, 18, 4, 226, 22, 21, 134, 476, 26, 480, 5, 144, 30, 2, 18, 51, 36, 28, 224, 92, 25, 104, 4, 226, 65, 16, 38, 2, 88, 12, 16, 283, 5, 16, 2, 113, 103, 32, 15, 16, 2, 19, 178, 32]
    

- Target 데이터 출력
  + 0은 부정리뷰
  + 1은 긍정리뷰


```python
print(train_target[:20])
```

    [1 0 0 1 0 0 1 0 1 0 1 0 0 0 0 0 1 1 0 1]
    

# 데이터셋 분리


```python
from sklearn.model_selection import train_test_split
train_input, val_input, train_target, val_target = train_test_split(train_input, train_target, test_size = 0.2, random_state = 42)

train_input.shape, val_input.shape, train_target.shape, val_target.shape
```




    ((20000,), (5000,), (20000,), (5000,))



# 데이터 시각화

- 각 리뷰의 평균단어의 갯수


```python
import numpy as np
# temp_list = [len(x) for x in train_input]
# print(temp_list)
lengths = np.array([len(x) for x in train_input])
print(np.mean(lengths), np.median(lengths))
```

    239.00925 178.0
    


```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

ax.hist(lengths)
ax.set_xlabel('length')
ax.set_ylabel('frequency')
fig.show()
```


    
![png](/images/Chapter_9/output_19_0.png)
    


- 짧은 단어 100개만 사용
- 모든 길이를 100에 맞춰줌
  + "패딩"
- 데이터의 갯수는 20000, 전체 길이는 100으로 맞춤


```python
from tensorflow.keras.preprocessing.sequence import pad_sequences
train_seq = pad_sequences(train_input, maxlen = 100)

print(train_seq.shape)
```

    (20000, 100)
    


```python
print(train_seq[5])
```

    [  0   0   0   0   1   2 195  19  49   2   2 190   4   2 352   2 183  10
      10  13  82  79   4   2  36  71 269   8   2  25  19  49   7   4   2   2
       2   2   2  10  10  48  25  40   2  11   2   2  40   2   2   5   4   2
       2  95  14 238  56 129   2  10  10  21   2  94 364 352   2   2  11 190
      24 484   2   7  94 205 405  10  10  87   2  34  49   2   7   2   2   2
       2   2 290   2  46  48  64  18   4   2]
    


```python
print(train_input[0][-10:])
print(train_seq[0])
```

    [6, 2, 46, 7, 14, 20, 10, 10, 470, 158]
    [ 10   4  20   9   2 364 352   5  45   6   2   2  33 269   8   2 142   2
       5   2  17  73  17 204   5   2  19  55   2   2  92  66 104  14  20  93
      76   2 151  33   4  58  12 188   2 151  12 215  69 224 142  73 237   6
       2   7   2   2 188   2 103  14  31  10  10 451   7   2   5   2  80  91
       2  30   2  34  14  20 151  50  26 131  49   2  84  46  50  37  80  79
       6   2  46   7  14  20  10  10 470 158]
    


```python
val_seq = pad_sequences(val_input, maxlen = 100)
```

# 순환 신경망 만들기


```python
from tensorflow import keras
model = keras.Sequential()
model.add(keras.layers.SimpleRNN(8, input_shape = (100, 500)))
model.add(keras.layers.Dense(1, activation = 'sigmoid'))
```

- 원핫 인코딩 적용


```python
train_oh = keras.utils.to_categorical(train_seq)
print(train_oh.shape)
```

    (20000, 100, 500)
    


```python
print(train_oh[0][0][:12])
```

    [0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 1. 0.]
    


```python
print(np.sum(train_oh[0][0]))
```

    1.0
    


```python
val_oh = keras.utils.to_categorical(val_seq)
```


```python
model.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     simple_rnn (SimpleRNN)      (None, 8)                 4072      
                                                                     
     dense (Dense)               (None, 1)                 9         
                                                                     
    =================================================================
    Total params: 4,081
    Trainable params: 4,081
    Non-trainable params: 0
    _________________________________________________________________
    


```python
rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model.compile(optimizer=rmsprop, loss='binary_crossentropy', 
              metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-simplernn-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

history = model.fit(train_oh, train_target, epochs=10, batch_size=64,
                    validation_data=(val_oh, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```

    Epoch 1/10
    313/313 [==============================] - 43s 125ms/step - loss: 0.7005 - accuracy: 0.5166 - val_loss: 0.6957 - val_accuracy: 0.5234
    Epoch 2/10
    313/313 [==============================] - 39s 124ms/step - loss: 0.6874 - accuracy: 0.5462 - val_loss: 0.6830 - val_accuracy: 0.5638
    Epoch 3/10
    313/313 [==============================] - 39s 124ms/step - loss: 0.6706 - accuracy: 0.5918 - val_loss: 0.6643 - val_accuracy: 0.6044
    Epoch 4/10
    313/313 [==============================] - 39s 124ms/step - loss: 0.6491 - accuracy: 0.6342 - val_loss: 0.6417 - val_accuracy: 0.6374
    Epoch 5/10
    313/313 [==============================] - 40s 129ms/step - loss: 0.6220 - accuracy: 0.6725 - val_loss: 0.6162 - val_accuracy: 0.6708
    Epoch 6/10
    313/313 [==============================] - 39s 126ms/step - loss: 0.5925 - accuracy: 0.7084 - val_loss: 0.5883 - val_accuracy: 0.7078
    Epoch 7/10
    313/313 [==============================] - 40s 128ms/step - loss: 0.5661 - accuracy: 0.7348 - val_loss: 0.5662 - val_accuracy: 0.7290
    Epoch 8/10
    313/313 [==============================] - 39s 124ms/step - loss: 0.5466 - accuracy: 0.7486 - val_loss: 0.5514 - val_accuracy: 0.7376
    Epoch 9/10
    313/313 [==============================] - 40s 127ms/step - loss: 0.5307 - accuracy: 0.7603 - val_loss: 0.5381 - val_accuracy: 0.7442
    Epoch 10/10
    313/313 [==============================] - 40s 128ms/step - loss: 0.5159 - accuracy: 0.7687 - val_loss: 0.5218 - val_accuracy: 0.7580
    

- 514p
  + 문제점 발생 : 토큰 1개를 500차원으로 늘림.. --> 데이터 크기가 500배 커짐


```python
from tensorflow import keras
model2 = keras.Sequential()
model2.add(keras.layers.Embedding(500, 16, input_length = 100))
model2.add(keras.layers.SimpleRNN(8))
model2.add(keras.layers.Dense(1, activation = 'sigmoid'))

model2.summary()
```

    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     embedding (Embedding)       (None, 100, 16)           8000      
                                                                     
     simple_rnn (SimpleRNN)      (None, 8)                 200       
                                                                     
     dense (Dense)               (None, 1)                 9         
                                                                     
    =================================================================
    Total params: 8,209
    Trainable params: 8,209
    Non-trainable params: 0
    _________________________________________________________________
    


```python
rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model2.compile(optimizer=rmsprop, loss='binary_crossentropy', 
              metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-simplernn-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

history = model2.fit(train_oh, train_target, epochs=10, batch_size=64,
                    validation_data=(val_oh, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```

    Epoch 1/10
    WARNING:tensorflow:Model was constructed with shape (None, 100) for input KerasTensor(type_spec=TensorSpec(shape=(None, 100), dtype=tf.float32, name='embedding_input'), name='embedding_input', description="created by layer 'embedding_input'"), but it was called on an input with incompatible shape (None, 100, 500).
    


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-16-865acaa6cf17> in <module>()
         10 history = model2.fit(train_oh, train_target, epochs=10, batch_size=64,
         11                     validation_data=(val_oh, val_target),
    ---> 12                     callbacks=[checkpoint_cb, early_stopping_cb])
    

    /usr/local/lib/python3.7/dist-packages/keras/utils/traceback_utils.py in error_handler(*args, **kwargs)
         65     except Exception as e:  # pylint: disable=broad-except
         66       filtered_tb = _process_traceback_frames(e.__traceback__)
    ---> 67       raise e.with_traceback(filtered_tb) from None
         68     finally:
         69       del filtered_tb
    

    /usr/local/lib/python3.7/dist-packages/tensorflow/python/framework/func_graph.py in autograph_handler(*args, **kwargs)
       1145           except Exception as e:  # pylint:disable=broad-except
       1146             if hasattr(e, "ag_error_metadata"):
    -> 1147               raise e.ag_error_metadata.to_exception(e)
       1148             else:
       1149               raise
    

    ValueError: in user code:
    
        File "/usr/local/lib/python3.7/dist-packages/keras/engine/training.py", line 1021, in train_function  *
            return step_function(self, iterator)
        File "/usr/local/lib/python3.7/dist-packages/keras/engine/training.py", line 1010, in step_function  **
            outputs = model.distribute_strategy.run(run_step, args=(data,))
        File "/usr/local/lib/python3.7/dist-packages/keras/engine/training.py", line 1000, in run_step  **
            outputs = model.train_step(data)
        File "/usr/local/lib/python3.7/dist-packages/keras/engine/training.py", line 859, in train_step
            y_pred = self(x, training=True)
        File "/usr/local/lib/python3.7/dist-packages/keras/utils/traceback_utils.py", line 67, in error_handler
            raise e.with_traceback(filtered_tb) from None
        File "/usr/local/lib/python3.7/dist-packages/keras/engine/input_spec.py", line 214, in assert_input_compatibility
            raise ValueError(f'Input {input_index} of layer "{layer_name}" '
    
        ValueError: Exception encountered when calling layer "sequential" (type Sequential).
        
        Input 0 of layer "simple_rnn" is incompatible with the layer: expected ndim=3, found ndim=4. Full shape received: (None, 100, 500, 16)
        
        Call arguments received:
          • inputs=tf.Tensor(shape=(None, 100, 500), dtype=float32)
          • training=True
          • mask=None
    


# LSTM 신경망 훈련하기

- RNN은 실모에서 안씀
- 나온 배경
  + 문장이 길면, 학습 능력이 떨어짐
  + Long Short-Term Memory
- 단기 기억을 오래 기억하기 위해 고안됨

## 데이터 불러오기


```python
from tensorflow.keras.datasets import imdb
from sklearn.model_selection import train_test_split

(train_input, train_target), (test_input, test_target) = imdb.load_data(
    num_words=500)

train_input, val_input, train_target, val_target = train_test_split(
    train_input, train_target, test_size=0.2, random_state=42)
```

    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/imdb.npz
    17465344/17464789 [==============================] - 0s 0us/step
    17473536/17464789 [==============================] - 0s 0us/step
    

## padding


```python
from tensorflow.keras.preprocessing.sequence import pad_sequences

train_seq = pad_sequences(train_input, maxlen=100)
val_seq = pad_sequences(val_input, maxlen=100)
```

## 모형 만들기


```python
from tensorflow import keras
model = keras.Sequential()
model.add(keras.layers.Embedding(500, 16, input_length = 100))
model.add(keras.layers.LSTM(8)) # SimpleRNN
model.add(keras.layers.Dense(1, activation = 'sigmoid'))

model.summary()
```

    Model: "sequential_2"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     embedding_2 (Embedding)     (None, 100, 16)           8000      
                                                                     
     lstm_2 (LSTM)               (None, 8)                 800       
                                                                     
     dense_2 (Dense)             (None, 1)                 9         
                                                                     
    =================================================================
    Total params: 8,809
    Trainable params: 8,809
    Non-trainable params: 0
    _________________________________________________________________
    


```python
rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model.compile(optimizer=rmsprop, loss='binary_crossentropy', 
              metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-lstm-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

history = model.fit(train_seq, train_target, epochs=10, batch_size=64,
                    validation_data=(val_seq, val_target),
                    callbacks=[checkpoint_cb, early_stopping_cb])
```

    Epoch 1/10
    313/313 [==============================] - 15s 41ms/step - loss: 0.5444 - accuracy: 0.7676 - val_loss: 0.5463 - val_accuracy: 0.7512
    Epoch 2/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.5286 - accuracy: 0.7750 - val_loss: 0.5313 - val_accuracy: 0.7592
    Epoch 3/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.5130 - accuracy: 0.7802 - val_loss: 0.5184 - val_accuracy: 0.7618
    Epoch 4/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.4985 - accuracy: 0.7836 - val_loss: 0.5060 - val_accuracy: 0.7654
    Epoch 5/10
    313/313 [==============================] - 12s 37ms/step - loss: 0.4875 - accuracy: 0.7887 - val_loss: 0.4986 - val_accuracy: 0.7692
    Epoch 6/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.4788 - accuracy: 0.7911 - val_loss: 0.4924 - val_accuracy: 0.7682
    Epoch 7/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.4708 - accuracy: 0.7952 - val_loss: 0.4829 - val_accuracy: 0.7782
    Epoch 8/10
    313/313 [==============================] - 12s 37ms/step - loss: 0.4644 - accuracy: 0.7948 - val_loss: 0.4800 - val_accuracy: 0.7768
    Epoch 9/10
    313/313 [==============================] - 11s 36ms/step - loss: 0.4590 - accuracy: 0.7988 - val_loss: 0.4734 - val_accuracy: 0.7852
    Epoch 10/10
    313/313 [==============================] - 11s 37ms/step - loss: 0.4535 - accuracy: 0.8011 - val_loss: 0.4721 - val_accuracy: 0.7792
    

## 손실 곡선 추가


```python
import matplotlib.pyplot as plt

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```


    
![png](/images/Chapter_9/output_46_0.png)
    


## 순환층에 드롭아웃 적용하기


```python
model2 = keras.Sequential()

model2.add(keras.layers.Embedding(500, 16, input_length=100))
# 드롭아웃 추가
model2.add(keras.layers.LSTM(8, dropout=0.3))
model2.add(keras.layers.Dense(1, activation='sigmoid'))

rmsprop = keras.optimizers.RMSprop(learning_rate=1e-4)
model2.compile(optimizer=rmsprop, loss='binary_crossentropy', 
               metrics=['accuracy'])

checkpoint_cb = keras.callbacks.ModelCheckpoint('best-dropout-model.h5', 
                                                save_best_only=True)
early_stopping_cb = keras.callbacks.EarlyStopping(patience=3,
                                                  restore_best_weights=True)

# epcohs = 100
history = model2.fit(train_seq, train_target, epochs=10, batch_size=64,
                     validation_data=(val_seq, val_target),
                     callbacks=[checkpoint_cb, early_stopping_cb])

plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.xlabel('epoch')
plt.ylabel('loss')
plt.legend(['train', 'val'])
plt.show()
```

    Epoch 1/10
    313/313 [==============================] - 15s 41ms/step - loss: 0.6926 - accuracy: 0.5304 - val_loss: 0.6920 - val_accuracy: 0.5602
    Epoch 2/10
    313/313 [==============================] - 12s 39ms/step - loss: 0.6905 - accuracy: 0.5802 - val_loss: 0.6892 - val_accuracy: 0.6224
    Epoch 3/10
    313/313 [==============================] - 12s 38ms/step - loss: 0.6849 - accuracy: 0.6448 - val_loss: 0.6795 - val_accuracy: 0.6624
    Epoch 4/10
    313/313 [==============================] - 12s 37ms/step - loss: 0.6551 - accuracy: 0.6910 - val_loss: 0.6247 - val_accuracy: 0.7226
    Epoch 5/10
    313/313 [==============================] - 12s 38ms/step - loss: 0.6070 - accuracy: 0.7212 - val_loss: 0.5985 - val_accuracy: 0.7214
    Epoch 6/10
    313/313 [==============================] - 12s 39ms/step - loss: 0.5844 - accuracy: 0.7358 - val_loss: 0.5752 - val_accuracy: 0.7462
    Epoch 7/10
    313/313 [==============================] - 12s 39ms/step - loss: 0.5645 - accuracy: 0.7509 - val_loss: 0.5551 - val_accuracy: 0.7572
    Epoch 8/10
    313/313 [==============================] - 12s 39ms/step - loss: 0.5443 - accuracy: 0.7605 - val_loss: 0.5372 - val_accuracy: 0.7640
    Epoch 9/10
    313/313 [==============================] - 12s 38ms/step - loss: 0.5263 - accuracy: 0.7671 - val_loss: 0.5214 - val_accuracy: 0.7710
    Epoch 10/10
    313/313 [==============================] - 12s 38ms/step - loss: 0.5108 - accuracy: 0.7762 - val_loss: 0.5049 - val_accuracy: 0.7762
    


    
![png](/images/Chapter_9/output_48_1.png)
    

