# 📨 IBM MQ: Installation & Basic Commands

![IBM MQ](https://img.shields.io/badge/IBM-MQ-052FAD?logo=ibm&logoColor=white)
![Middleware](https://img.shields.io/badge/Messaging-Middleware-orange)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)

Hands-on guides from working with **IBM MQ**, the enterprise messaging middleware, in production environments.

| Document | Contents |
|----------|----------|
| 📄 [MQ SERVER INSTALLATION.pdf](MQ%20SERVER%20INSTALLATION.pdf) | Step-by-step MQ server installation on Linux |
| 📄 [MQ BASIC COMMANDS.pdf](MQ%20BASIC%20COMMANDS.pdf) | Day-to-day administration commands |

## What is IBM MQ?

IBM MQ (formerly *MQSeries*) lets applications exchange messages **reliably and asynchronously** through queues. Sender and receiver don't need to be online at the same time, and messages are never lost or duplicated. It is the backbone of integration in banking, telecoms and retail.

### Key features

- ✅ **Reliable, exactly-once delivery** with transactional integrity (local and XA)
- ⏱️ **Asynchronous communication** that decouples producers and consumers
- 💾 **Persistent messages** that survive restarts and outages
- 🔐 **Security:** authentication, authorisation (OAM), TLS in transit, encryption at rest
- 📬 **Queue types:** local, remote, transmission, alias, model and dead-letter queues
- 📡 **Publish/subscribe** for event-driven architectures
- 📈 **Scalability:** clustering, workload balancing, high availability (RDQM / multi-instance)
- 🧩 **APIs:** MQI, JMS, REST, plus Java, C, .NET and Python clients
- 🛠️ **Tooling:** MQ Explorer, MQ Console, `runmqsc`

## Cheat sheet

```bash
# Queue manager lifecycle
crtmqm QM1                 # create
strmqm QM1                 # start
dspmq                      # list queue managers + status
endmqm -i QM1              # stop (immediate)
dltmqm QM1                 # delete

# Administration with MQSC
runmqsc QM1
```

```text
DEFINE QLOCAL('APP.IN') MAXDEPTH(5000)
DEFINE CHANNEL('APP.SVRCONN') CHLTYPE(SVRCONN)
DEFINE LISTENER('L1414') TRPTYPE(TCP) PORT(1414) CONTROL(QMGR)
START LISTENER('L1414')
DISPLAY QLOCAL('APP.IN') CURDEPTH
DISPLAY CHSTATUS(*)
ALTER QMGR CHLAUTH(ENABLED)
END
```

```bash
# Test put/get with the samples
/opt/mqm/samp/bin/amqsput APP.IN QM1
/opt/mqm/samp/bin/amqsget APP.IN QM1

# Permissions (OAM)
setmqaut -m QM1 -t qmgr -g appgroup +connect +inq
setmqaut -m QM1 -n APP.IN -t queue -g appgroup +put +get +browse
```
