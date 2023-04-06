[[2023-04-06]] #Game 

### Repeated Games
[[16 Game Theory Basics#^731cea|Nash equilibrium]] can leave both players **worse off** relative to some other outcome. To reach that other outcome, players need to **coordinate**.  

```ad-important
**Definition 17.1**: Repeated Games

Repeated games are simultaneous games that are played more than once by  
the same players. Provide players with opportunity to **tacitly coordinate** with one another and reach better outcomes.  
```

There are two types: infinite and finite.

#### Tacit Collusion in Infinitely Repeated Games
Consider the following example

```ad-example
Consider following strategy: Shell signals to play “High” forever, as long as BP plays “High” forever. If BP plays “Low” at any point in the game, Shell will respond by playing “Low” in every subsequent round of the game. BP signals to do the same.

Then, BP's payoff from cooperating is $$X_{\text{Coop}}=\sum\limits_{i=0}^{T} \frac{x_{\text{Coop}}}{(1+r)^{t}}=x_{\text{Coop}}=x_{\text{Coop}}+\frac{x_{\text{Coop}}}{r}$$

BP's payoff from deviating in any round is $$X_{\text{Dev}}=\sum\limits_{i=1}^{T} x_{\text{Dev}}+\frac{x_{\text{Punish}}}{(1+r)^{t}}=x_{\text{Coop}}=x_{\text{Dev}}+\frac{x_{\text{Punish}}}{r}$$
```

Obviously, it's worth cooperating if $$x_{\text{Coop}}+\frac{x_{\text{Coop}}}{r}>x_{\text{Dev}}+\frac{x_{\text{Punish}}}{r}$$
Otherwise, we fall back to the Nash Equilibrium. This strategy is known as a **grim trigger**.