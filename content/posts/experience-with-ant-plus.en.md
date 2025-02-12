+++ 
draft = false
date = 2025-02-11T21:14:01-03:00
started_date = 2025-01-21T21:14:01-03:00
title = "Challenges and Lessons Learned in Using ANT+ for Heart Rate Monitoring"
description = ""
slug = ""
authors = ["Ramon Ribeiro"]
tags = ["javascript", "React", "Electron"]
categories = ["Case Study"]
externalLink = ""
series = []
+++

![Thumbnail](/images/antplus.png)  

A few years ago, I took on an interesting challenge: developing a desktop application that required using a technology I had never experimented with before. That technology was [ANT+](https://www.thisisant.com/).  

After a quick Google search, I realized that ANT+ was quite similar to Bluetooth, which I was already familiar with. This gave me the confidence to tackle the project head-on.  

# About the App  

The goal of the application was to create a dashboard for a gym, allowing trainers to monitor students' heart rates during workouts. With this data, the personal trainer could track real-time performance and provide immediate support to students.  

The app would also be accessible to the students themselves, enabling them to track metrics such as calories burned and interact dynamically with the instructor.  

# About the Devices  

For the ANT+ devices, I used a [USB dongle](https://www.amazon.com.br/CooSpo-extens%C3%A3o-compat%C3%ADvel-TrainerRoad-Sufferfest/dp/B07CB4328P?th=1) to provide connectivity to the computer. The dongle supports up to 8 communication channels.  

![USB dongle](https://m.media-amazon.com/images/I/71FJqTisEqL._AC_SX522_.jpg)  

The other device is the [Magene H64](https://www.amazon.com.br/CINTA-CARD%C3%8DACA-MAGENE-H64-BLUETOOTH/dp/B08XK71V4R) heart rate monitor strap, which supports Bluetooth 4.0 and ANT+.  

![Magene H64](https://m.media-amazon.com/images/I/61H7YdeTANL._AC_SX679_.jpg)  

# Challenges Faced  

Beyond the challenge of learning a new technology, one of the biggest hurdles was finding proper documentation on what I needed to get started.  

I first considered the system's requirements: since it needed to run on a desktop rather than a web-based system (which I was more experienced in developing), I had to look for technologies that provided good support for desktop applications.  

I quickly thought of the [Electron](https://www.electronjs.org/) framework, which would allow me to leverage my skills in HTML and CSS while converting them into a desktop format.  

Still, I wanted to make use of my experience with [React](https://react.dev/), which would allow me to easily create pages using its [JSX](https://legacy.reactjs.org/docs/introducing-jsx.html) syntax.  

However, I knew that setting up communication between React and Electron from scratch could be time-consuming. So, I searched for an open-source boilerplate repository. To my surprise, I found a well-rated one on GitHub: [electron-react-boilerplate](https://github.com/electron-react-boilerplate/electron-react-boilerplate).  

With that, I finalized my frontend architecture.  

The hardest part was finding a library to send and receive ANT+ commands. I couldn't find an official ANT+ API for JavaScript, so I had to rely on third-party libraries, which worried me because there wasn't a strong developer community for support.  

I tested multiple libraries available on npm, and after several attempts, I finally found one that met my expectations, allowing me to send and receive data from the heart rate strap.  

# Technologies Used  

## ANT+  

As mentioned, ANT+ is a wireless communication technology similar to Bluetooth, but it focuses on fitness and wellness applications, whereas Bluetooth is designed for general device communication.  

A key difference is that ANT+ is more energy-efficient. Even with newer Bluetooth versions like BLE (Bluetooth Low Energy), ANT+ can still be more efficient in certain scenarios. Additionally, ANT+ supports multi-device connectivity, allowing multiple devices to connect simultaneously.  

Since Bluetooth is a more general communication protocol, it has a broader developer community compared to ANT+. The lack of documentation in ANT+ makes development more challenging.  

Unlike Bluetooth, which requires a connection request (handshaking), ANT+ works with broadcast channels. A receiving device just needs to tune in to the correct channel to receive data.  

You might wonder: what if two devices use the same channel? ANT+ manages this efficiently by alternating channels as needed.  

### Using the [gd-antplus](https://github.com/gdoumen/ant-plus) Library  

The library has two main classes that I used: **GarminStick2** and **HeartRateSensor**.  

- **GarminStick2**: abstracts the use of the USB dongle, checking if it is connected and ready to listen to channels.  
- **HeartRateSensor**: responsible for interpreting the data transmitted by the heart rate strap.  

I implemented an interface where the personal trainer could associate each student with a monitoring device. During the initial workout setup, the trainer would make this association, allowing real-time metric calculations.  

### Energy Expenditure Calculation  

Besides displaying the real-time heart rate, the system also calculated energy expenditure. The formula from [Keytel et al. (2005)](https://www.researchgate.net/publication/7777759_Prediction_of_energy_expenditure_from_heart_rate_monitoring_during_submaximal_exercise) was used.  

For men:  

$$
DE (kcal) = \frac{-55.0969 + 0.6309 \times FC (bpm) + 0.1988 \times peso(kg) + 0.2017 \times idade}{4.184 \times 60 \times tempo (min)}
$$  

For women:  

$$
DE (kcal) = \frac{-20.4022 + 0.4472 \times FC (bpm) + 0.1263 \times peso(kg) + 0.074 \times idade}{4.184 \times 60 \times tempo (min)}
$$  

where:  
- **DE** = Energy Expenditure  
- **FC** = Heart Rate  

# Final Results  

![ANT+ Result](/images/resultado-antplus.png)  

After implementing both the frontend and backend, I successfully delivered a functional proof of concept to the client at the gym. The system operated as expected, enabling real-time heart rate monitoring.  

Unfortunately, due to budget limitations and the gym's infrastructure, the project was not carried forward. However, the experience provided me with valuable learning about new technologies, especially ANT+, React, and Electron.  

Even without a commercial implementation, the knowledge gained was worth every line of code written.  
