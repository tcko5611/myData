# Tcp

## QTcpServer

The sample code for thread is like:

```
#ifndef MYTHREAD_H
#define MYTHREAD_H

#include <QThread>
#include <QTcpSocket>
#include <QDebug>

class MyThread : public QThread
{
    Q_OBJECT
public:

    explicit MyThread(qintptr ID, QObject *parent = 0);
// open socket
// connect ocket readRead, socket  disconnected signals, 
// enter event loop with exec()
    void run();

signals:
    void error(QTcpSocket::SocketError socketerror);
    
public slots:
// read data and write data
    void readyRead();
	// when disconnected , deleteLater self
    void disconnected();

private:
    QTcpSocket *socket;
    qintptr socketDescriptor;
};

#endif // MYTHREAD_H

```

The sample code for QTcpServer is like:

```
// myserver.h

#ifndef MYSERVER_H
#define MYSERVER_H

#include <QTcpServer>

class MyServer : public QTcpServer
{
    Q_OBJECT
public:
    explicit MyServer(QObject *parent = 0);
    void startServer(); // listen to server, port
signals:
    
public slots:
    
protected:
// start listen thread and connect thread finished to  deleteLater
    void incomingConnection(qintptr socketDescriptor);
     
};

#endif // MYSERVER_H

```

## QTcpSocket for client

The sample code for client is like:

```
#ifndef MYTCPSOCKET_H
#define MYTCPSOCKET_H

#include <QObject>
#include <QTcpSocket>
#include <QDebug>

class MyTcpSocket : public QObject
{
    Q_OBJECT
public:
    explicit MyTcpSocket(QObject *parent = 0);
    
    void doConnect();

signals:
  void finished(); // for finished, connect to app.quit()
public slots:
  void connected(); // for socket connected, start talking
  void readyRead(); //  for socket data ready to read
private:
    QTcpSocket *socket;
    
};

```

It's interesting client work as a listener to socket, so it need to enter event loop by QCoreApplication.
