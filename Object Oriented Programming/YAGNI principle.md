> Always implement things when you actually need them, never when you just foresee that you might need them.

The rationale behind YAGNI is simple: ==every line of code we write comes with a cost. It needs to be developed, tested, maintained, and understood by other developers.==

By adding unnecessary features or over-engineering solutions, we incur additional complexity, which can slow down development, increase the likelihood of bugs, and make the codebase harder to maintain..
# Examples
## **Example: Payment Processing**

**Over-engineered:**

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1026e117-5d09-46ab-afbb-67d046f98985_1302x370.png)


**YAGNI-aligned:**

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb2a83e62-b99c-4760-b929-567ca0c20544_1110x204.png)

Start by supporting only the payment methods you _currently_ need. Add support for PayPal or Bitcoin later in the development cycle if the demand arises.
# Benefits

1. **Reduced waste**: By only implementing what's necessary, you avoid wasting time and effort on unnecessary code.
    
2. **Simplified codebase**: YAGNI promotes a lean and simple codebase, making it easier to maintain and update.
    
3. **Faster development**: A laser focus on immediate needs means you get features out the door faster. You're not bogged down by potential "what ifs".
    
4. **Improved adaptability**: With a flexible and modular codebase, you can respond quickly to changing requirements.

# **When YAGNI Might Be Inappropriate**

Like any principle, YAGNI shouldn't be rigidly applied in every situation. There are times when anticipating near-future needs makes sense:

- **Well-Known Requirements:** If you know with high certainty a feature is coming soon, building some basic support upfront might be wise.
    
- **Performance-Critical Areas:** Sometimes, a less-than-optimal but more general solution is necessary initially to ensure performance targets are met.
