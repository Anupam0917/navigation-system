# navigation-system
 A hybrid AI + inertial navigation architecture in which the smartphone acts as a self-contained dead-reckoning sensor platform. GNSS is used whenever it is reliable, while the system continuously maintains an inertial estimate in the background so that a GNSS outage does not create a discontinuity.
1. Collect accelerometer, gyroscope, magnetometer/compass and GNSS data when available.
2. Clean, synchronize and calibrate the sensor streams; detect abnormal measurements and unwanted phone movement.
3. Estimate the phone-to-vehicle orientation so the system can transform phone-frame motion into a vehicle/road frame.
4. Use lightweight AI/ML models to recognize motion patterns, estimate forward speed/velocity where feasible, and estimate sensor/measurement reliability.
5. Feed IMU prediction, AI-derived information, GNSS (when available), vehicle constraints and map information into an IEKF/EKF-based fusion engine.
6. When GNSS is unavailable, continue inertial dead reckoning and use map matching and kinematic constraints to reduce physically implausible drift.
7. When the vehicle stops, use the stop as a possible correction opportunity to compare the inertial trajectory against map-consistent geometry and estimate accumulated drift.
8. Monitor AI confidence; if AI becomes unreliable, fall back to the classical IEKF/filter path rather than allowing a low-confidence AI output to dominate.
9. When GNSS returns, blend/reinitialize the fusion state smoothly instead of visually jumping the vehicle marker.

    ARCHITECURE
   --------------
           Smartphone IMU
   Accelerometer + Gyroscope
              │
              ▼
       ┌───────────────┐
       │      AI       │
       │ Intelligence  │
       │    Layer      │
       └───────┬───────┘
               │
       ┌───────┴────────┐
       │                │
       ▼                ▼
 Motion estimation   Sensor confidence
 Speed / motion      Noise / covariance
 classification      estimation
       │                │
       └───────┬────────┘
               ▼
             IEKF
               │
     ┌─────────┼─────────┐
     ▼         ▼         ▼
   GNSS       Map      Vehicle
  when avail  data    constraints
     │         │         │
     └─────────┼─────────┘
               ▼
        Position + Heading
        + Velocity + Confidence




   
