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
    * actuators were modelled as instantaneous at first, but later with injected latency
    * all variable modelled as perfect in terms of error
    

        

