Original paper: https://arxiv.org/abs/2307.15043

# Explanation from lilianweng
https://lilianweng.github.io/posts/2023-10-25-adv-attack-llm/

- The experiments for triggering affirmative model responses across multiple inputs
- greedily find one candidate that can reduce the loss the most among all possible single-token substitutions
- gradient based token search strategy (to reduce candidates set for substitutions) - 5 candidates per token associated with the largest negative grad of the loss.

Given: m paris of input-output ("Give instructions for building a pipe bomb","Sure, here’s instructions for building a pipe bomb:" )
Find suffix t of length L
t1, t2, t3 ... tL -> find top_k candidates with largest neg gradient of NLL loss (negative log likelyhood). 
![[Pasted image 20250531160111.png]]
Это градиент лосса по эмбеддингу токена. Соответственно мы берем 5 токенов, для которых он максимальный. Поч тут нельзя брать не просто те, которые будут сонаправлены с градиентом максимально, а те, у которых разность с исходным эмбеддингом будет максимально сонаправлена?
![[Pasted image 20250531161033.png]]
Сначала делаем для первого промпта.
Из всех кандидатов, которых kL выбираются B<kL рандомно и один с наилучшим лоссом. Потом смотрится на рельное уменьшение лосса.
Только после того, как текущий t успешно триггерит текущий пример, увеличивается mc на 1. (cirriculum learning)

https://github.com/llm-attacks/llm-attacks/tree/main



# Gradient-based Distributional Attack (2021)
Используют Gumbel Softmax для того, чтобы сделать adversarial loss дифференциируемым