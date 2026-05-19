+++
date = 2026-05-19
title = 'A note on PID control'
draft = false
math = true
+++

The controller acts on the tracking error \(e(t) = r(t) - y(t)\).

$$u(t) = K_p\, e(t) + K_i \int_0^t e(\tau)\, d\tau + K_d \frac{d e(t)}{dt}$$