# **Честный Proof-of-Stake**
С самого начала Waves использовала «нетронутую» модель Proof of Stake (PoS) (подробнее о PoS вы можете прочитать здесь 
[/platform-features/leased-proof-of-stake-lpos.md)), предложенную [Nxt] (https://nxtwiki.org/wiki/Whitepaper:Nxt). 
В этой модели выбор аккаунта, который имеет право генерировать следующий блок и получать соответствующие транзакционные сборы, основан на количестве токенов у учетной записи. Чем больше у аккаунта токенов, тем больше вероятность того, что аккаунт получит право на создание блока.

Мы в Waves убеждены, что каждый участник блокчейна должен участвовать в процессе генерации блоков пропорционально своей доле владения: мы решили исправить формулу PoS. На данный момент у нас нет цели полностью изменить алгоритм, поскольку нет такой необходимости; мы просто хотим внести некоторые коррективы.

in accordance with the shortcomings of the current algorithm. We analyzed the model of the new algorithm for its correspondence to the stake share and the share of blocks, and the results were positive. Also, the algorithm was analyzed for vulnerability to attacks, and results obtained with the new model were better than with the old one. The attacks’ results for the attacker were not so successful in terms of the profits gained. The number of forks and their length decreased. 

Мы представляем улучшенный алгоритм PoS, который делает выбор создателя блока справедливым и снижает уязвимость к multi-branching атакам. Мы проанализировали модель нового алгоритма на соответствие доли владения и доли созданных блоков, и результаты являются положительными. Кроме того, алгоритм проанализирован на предмет уязвимости к атакам, и результаты, полученные с применением новой модели лучше, чем полученные с применением старой модели. Результаты атаки для атакующего теперь не столь успешны с точки зрения полученной прибыли. Количество форков и их длина уменьшились.

Честный PoS назначен на релиз [0.13.3](https://github.com/wavesplatform/Waves/releases).

**Примечание.** Вы можете найти больше технической информации о наших улучшениях алгоритма PoS [**в этой статье**](https://forum.wavesplatform.com/uploads/default/original/1X/b9f220c13f73c3a41dff7f4523c6c4a1fc03ebf6.pdf).