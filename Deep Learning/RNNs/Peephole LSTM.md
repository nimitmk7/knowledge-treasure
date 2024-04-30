Peephole LSTM is a slight refinement of vanilla LSTM that grants the gates more information for their decision making. That more information is the **cell state**.

| Feature                 | Vanilla LSTM                         | Peephole LSTM                                    |
| ----------------------- | ------------------------------------ | ------------------------------------------------ |
| Gate Information Source | Previous Hidden State, Current Input | Previous Hidden State, Current Input, Cell State |

## Changed Architecture and Equations:


![[Pasted image 20240429144658.png]]

The key difference is the addition of peephole connections (red lines) from the cell state ($C_{t-1}$) to all three gates.

The intuition is that this potentially might allow the gates to make more informed decisions by considering the current memory content.


