# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## Discussion questions

* The model used for this project consisted of:
    * an 4d state vector x consisting of x-position, y-position, orientation, and velocity
    * augmentations of cross-track error and orientation error to the previous, yielding a 6d vector
    * a 2d control vector for our actuators, the steering angle, delta, and the "acceleration", alpha
    * a parameter, Lf, the distance from the center of gravity to the front of the vehicle 
    * purely kinematic update equations:
        * x = x + v * cos(psi)dt
        * y = y + y * sin(psi)dt
        * psi = psi + (v/Lf) * delta * dt
        * v = v + a * dt
        * cte = f(x) - y + (v * sin(epsi) * dt)
        * epsi = psi - psi_desired + ((v/Lf) * delta * dt)
    * this model with given parameter was verified via constant v, constant heading driving of the simulated car in a circular path
    * state variables modeled as discrete, with instantaneous change at each timestep
    * actuators were modelled as instantaneous at first, but later with injected latency; also, as distinctly nonholonomic
    * all variable modelled as perfect in terms of error
    

* Tuning of the time parameters was as follows:
    * The suggestion to keep T, i.e. dt * N, to within the range of a few seconds for automotive-domain problems was followed.
    * For the no-latency testing, 10 steps of 0.1 seconds proved more or less adequate. Drove fairly smoothly, but only at about 60% target velocity on average. Extending this forward considerably was not productive in terms of performance vs computational heft on my laptop.
    * With the introduction of the 100 ms latency, the car became extremely unstable, veering of the road fairly quickly, turning the velocity way down helped this, but was an unsatisfying solution, and no range of dt, N values proved adequate to fix the issue.
    * After introducing model changes to compensate the latency (see below), I found that, while my original dt, N were still nonfunctional, raising the dt above the expected latency very quickly improved the trajectory of the car. Further, I found .2 seemed to sit on a rough peak, as even 0.05 changes up or down were detrimental. Finally, I found that I was able to gain some consistency in my results by pulling back on the number of time steps (to 8 from 10).
    * Ultimately, the car rarely manages more than 60-70MPH, and I would like to spend time later on further tuning my cost weights and the horizon parameters to see if I can get better speed.
    
* Latency
    * At the suggestion of the Udacity help video, I simulated the initial position of the car forward via it's kinematic equations by the expected latency prior to passing it in as an initial value. This was immediately quite successful, though as mentioned previously, I suspect a bit of tuning could yield better speeds. I'm also interested in later modifying the transition functions to accound for this directly if possible, I suspect this would become quickly intricate in terms of the higher level view of control vs actual effect at each step, etc.. But the crazy modeling is always so fun ;-)
    * We also had the benefit of essential precise, constant, and linear latencies. I'd be very much interested to see how people have made progress with more realistic actuator models.
    
* End thoughts
    * I found myself feeling a bit too dependent on some of the provided code (in particular the structural/ordering construction of the vectors to be passed in), which ultimately leaves a bit of a satisfaction void as I near the end. I do however understand that this was natural as the project was so strongly based around very abstract and demanding API usages, whereas the actual vehicle modelling and hand-updating was fairly straight-forward. 
    * All that said, I found the material itself among the most interesting of the course so far, and I intend to toy around with my model extensively in terms of the aforementioned tuning opportunities, as well as model changes (dynamic factors, error in actuation and sensing, driving performance optimization). I think as a fan of auto racing, you've got to be really happy working with MPC in this kind of problem :-D
    * I feel like learning to use the given APIs may well be one of the most useful things for my future career, MPC is such a broad tool, and given that it accepts such a wide range of model complexities, I really feel like I now have the ability to formulate and code solutions to problems that far outperform the very basic tools I had for the same problem prior to this section. 