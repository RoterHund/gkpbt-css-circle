# Readme
To create a model where rewards distributed to each validator are progressive, meaning the average reward rate decreases monotonically with increasing staked amount, we need a function that assigns rewards in such a way that validators with more staked tokens receive proportionally less in terms of reward rate. Here's a detailed proposal for such a model:

### Model Proposal

1. **Definitions:**
   - Let \( n \) be the number of validators.
   - Let \( X \) be the total amount of tokens staked across all validators.
   - Let \( Z(i) \) be the amount of tokens staked by validator \( i \) (where \( i \) ranges from 1 to \( n \)).
   - Let \( Y \) be the total amount of tokens to be distributed as rewards across all validators at the end of an epoch.

2. **Progressive Reward Function:**
   To ensure that the reward rate decreases as the staked amount increases, we define a reward function \( f(Z(i)) \) that assigns rewards to validators based on their staked amount. This function should be concave and decreasing in its derivative. A simple function that satisfies this requirement is a power-law function with an exponent less than 1.

   We propose the reward function \( f(Z(i)) = \left( \frac{Z(i)}{X} \right)^{\alpha} \), where \( 0 < \alpha < 1 \).

3. **Normalization:**
   The rewards should sum up to the total reward \( Y \). Hence, we need to normalize the function to ensure this condition is met. 

   The rewards for validator \( i \) can be given by:
   \[
   R(i) = \frac{\left( \frac{Z(i)}{X} \right)^{\alpha}}{\sum_{j=1}^{n} \left( \frac{Z(j)}{X} \right)^{\alpha}} \times Y
   \]

4. **Reward Rate:**
   The reward rate for validator \( i \) is:
   \[
   r(i) = \frac{R(i)}{Z(i)} = \frac{\left( \frac{Z(i)}{X} \right)^{\alpha}}{Z(i) \sum_{j=1}^{n} \left( \frac{Z(j)}{X} \right)^{\alpha}} \times Y
   \]

### Explanation:
- The power-law function \( \left( \frac{Z(i)}{X} \right)^{\alpha} \) ensures that the reward increases with the staked amount but at a decreasing rate due to \( 0 < \alpha < 1 \).
- The normalization ensures that the total rewards distributed sum up to \( Y \).
- The reward rate \( r(i) \) is inversely related to \( Z(i) \), ensuring a progressive distribution.

### Example:
Assume there are three validators with the following stakes:
- \( Z(1) = 10 \)
- \( Z(2) = 20 \)
- \( Z(3) = 30 \)

Total stake \( X = 60 \) and total rewards \( Y = 100 \). Let \( \alpha = 0.5 \).

1. Calculate the individual components:
   \[
   \left( \frac{Z(1)}{X} \right)^{\alpha} = \left( \frac{10}{60} \right)^{0.5} = \left( \frac{1}{6} \right)^{0.5} \approx 0.408
   \]
   \[
   \left( \frac{Z(2)}{X} \right)^{\alpha} = \left( \frac{20}{60} \right)^{0.5} = \left( \frac{1}{3} \right)^{0.5} \approx 0.577
   \]
   \[
   \left( \frac{Z(3)}{X} \right)^{\alpha} = \left( \frac{30}{60} \right)^{0.5} = \left( \frac{1}{2} \right)^{0.5} \approx 0.707
   \]

2. Sum the components:
   \[
   \sum_{j=1}^{n} \left( \frac{Z(j)}{X} \right)^{\alpha} \approx 0.408 + 0.577 + 0.707 = 1.692
   \]

3. Calculate the rewards:
   \[
   R(1) = \frac{0.408}{1.692} \times 100 \approx 24.12
   \]
   \[
   R(2) = \frac{0.577}{1.692} \times 100 \approx 34.12
   \]
   \[
   R(3) = \frac{0.707}{1.692} \times 100 \approx 41.76
   \]

4. Calculate the reward rates:
   \[
   r(1) = \frac{24.12}{10} = 2.41
   \]
   \[
   r(2) = \frac{34.12}{20} = 1.71
   \]
   \[
   r(3) = \frac{41.76}{30} = 1.39
   \]

As desired, the reward rate decreases as the amount staked increases, ensuring a progressive reward distribution.
