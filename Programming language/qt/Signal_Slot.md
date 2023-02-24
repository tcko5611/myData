# Signal Slot

## Basic mechanism

A simple code for signal and slot is like follows:

```
#include <QObject>
class Counter : public QObject
{
    Q_OBJECT
public:
    Counter() { m_value = 0; }
    int value() const { return m_value; }

public slots:
    void setValue(int value);

signals:
    void valueChanged(int newValue);

private:
    int m_value;
};

```

When use signal, it will need _moc_ to generate signal information. The slot functions are general member functions.

```
connect(sender, SIGNAL(destroyed(QObject*)), this, SLOT(objectDestroyed(Qbject*)));

```

## use make and cmake to generate moc, qrc, ui files

when use make to generate moc, qrc, ui files, we need to write the following code in Makefile

```
QT_MOCSOURCE = $(addprefix moc_, $(addsuffix .cpp, $(basename $(QT_MOCFILE))))
QT_RCCSOURCE = $(addprefix qrc_, $(addsuffix .cpp, $(basename $(QT_RCCFILE))))
QT_UICSOURCE = $(addprefix ui_, $(addsuffix .h, $(basename $(QT_UICFILE))))

```

when use cmake, we just need to add the following line in CMakeLists.txt

```
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

```

## Where is the thread that slot functions evaluate?

In the end of connect, we can add connection type like:

```
connect(socket, SIGNAL(readyRead()), this, SLOT(readyRead()), Qt::DirectConnection);

```

The last argument is about how the slot is executed in which thread, there 4 types:

-   Qt::AutoConnection 0 (default) If the signal is emitted from a different thread than the receiving object, the signal is queued, behaving as Qt::QueuedConnection. Otherwise, the slot is invoked directly, behaving as Qt::DirectConnection. The type of connection is determined when the signal is emitted.
-   Qt::DirectConnection 1 The slot is invoked immediately, when the signal is emitted.
-   Qt::QueuedConnection 2 The slot is invoked when control returns to the event loop of the receiver's thread. The slot is executed in the receiver's thread.
-   Qt::BlockingQueuedConnection 4 Same as QueuedConnection, except the current thread blocks until the slot returns. This connection type should only be used where the emitter and receiver are in different threads.

It's especially import for GUI and Nerwork problems.



