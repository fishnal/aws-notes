## General Advice
- Scale back slowly to ensure that additional computing power (added from scaling events) do not get destroyed too quickly, because the additional resources could be used again momentarily, and it can take a long time to initialie more instances
- Set long cooldown periods between scaling events

**Trigger points** indicate when a resource should scale, and how it should scale
- Created via **CloudWatch**
- Should it scale vertically or horizontally?
- Are more or less resources needed?

## Types of Scaling

### Step Scaling
- Horizontally scale based on performance metrics (i.e. cpu usage)
- Can add OR remove instances if demand is too high or low respectively
- Utilizes a **cooldown policy** which indicates how long to wait before re-evaluating performance thresholds
- Useful in case a new instance is added to handle high demand, but it
takes a few minutes for the instance to be ready. If scaling system did
not have this cooldown policy, it would immediately still see thresholds
being violated and attempt to add more instances, even though there was
already an instance that's starting up.
- If a scaling policy is triggered and **another scaling event is on-going**, then the
scaling policy will adjust to add/remove instances accordingly.
	- Suppose the current scaling event is planning to add 3 instances
	- A 2nd scaling policy is triggered and wants to add 5 instances.
	- Since a scaling event is on-going and will add 3 instances, the 2nd scaling policy will only add 2 instances
	- Scaling policies will only add/remove instances in order to achieve it's target number of instances; and will take into account current on-going scaling events.

### Target Tracking
- Can specify a target threshold to stay within (i.e. stay within 20%-60% cpu usage)
- More convenient than **step scaling** because you don't have to define all of the scaling policies; target tracking does that for you
- Don't delete scaling policies for target tracking

### Predictive Scaling
- Dynamically updates scaling policies to change based on a prediction model
- Goal is to scale ahead of time, before traffic actually spikes
- Uses machine learning to predict traffic changes in the future.
- Uses metrics from CloudWatch in making predictions
- Useful for a service that receives traffic on a cycle (i.e. normal business hours)

### Scheduling Scaling
- Schedule when a scaling event should occur (i.e. at night time, at morning, every other day, etc)
- Best for very predictable or known workloads
- Can be good for turning off development environments at night, and have them turn back in the morning
