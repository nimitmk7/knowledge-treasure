We deploy the new model in parallel with the existing model. Each incoming request is routed to both models, but only the existing model’s prediction is served to the user.

This minimizes the risk of unreliable predictions until the newly developed model has been thoroughly tested. 

But, it is a costly approach as it doubles the number of predictions.

![[shadow_deployment.png | 300]]

