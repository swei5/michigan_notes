[[2024-10-01]] #Derivatives #Valuation 

```ad-important
**Definition 11.1**: Options

A European call (put) option is a contract giving the holder the right to buy (sell) the underlying asset for a fixed price $X$ at a specified future time $T$.

An American call (put) option is a contract giving the holder the right to buy (sell) the underlying asset for a fixed price $X$ at **any time between now and a future time** $T$.

The price $X$ is called the **strike price** or exercise price.

The time $T$ is called the **exercise time** or expiry time.

Both $X$ and $T$ are set today. The person who buys the option is called the **holder** and the person who sells the option is called the **writer**.
```

The payoff of a call option at time $T$ is $$\max(S(T)-X,0)$$ also denoted by $(S(T)-X)^+$. The payoff of a put option at time $T$ is $$\max(X-S(T),0)$$ also denoted by $(X-S(T))^{+}$.

```ad-note
The payoffs are **non-negative random variables**. This gives the holder’s position an advantage over the position of the writer. So the holder has to **pay** the writer an amount at time $0$ in order to buy the option.
```
