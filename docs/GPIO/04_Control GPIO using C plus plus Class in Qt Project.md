---
sidebar_position: 18
---

# Control GPIO using C++ Class in Qt Project
### **Qt - GPIO Button**

Please reference to the Repository :

https://github.com/yourskc/q563_rzgpio/

The most important concept in this project is how to **link QML to our C++ class**. All **GPIO control functions through Linux driver are all inside the C++ class**.

The below are some important concepts in the above programs.

### **Register a C++ class to use from QML**

If you need to register a C++ class to use from QML, you can declaring your **QQmlApplicationEngine** and assign it as the parent of your object.

Reference :

[QQmlEngine Class | Qt Qml 6.8.1](https://doc.qt.io/qt-6/qqmlengine.html#qmlRegisterType)

Once this is registered, the type can be used in QML.

**# tip**

The above reference about QQmlApplicationEngine might not applicable to **Qt5.6.3**. However, the source code below has been verified on Qt5.6.3, please use them directly.

**main.cpp :**

```
#include <rzgpio.h>
...
int main(int argc, char *argv[])
{
QGuiApplication app(argc, argv);
QQmlApplicationEngine engine;

RzGPIO* rzgpio = new **RzGPIO**(&engine);
QQmlContext* context = engine.rootContext();
context->setContextProperty("RzGPIO", rzgpio);

engine.load(QUrl(QStringLiteral("qrc:/main.qml")));

return app.exec();
}
```

**main.qml :**

```
Button {
x:70
y:200
...
onClicked : {
        if (thismain.led2 === 0) {
            RzGPIO.Write(2, 1);
            thismain.led2=2;
            }
            else {
                **RzGPIO**.Write(2, 0);
                thismain.led2=0;
                }

            }
```

**QTimer** :

Reference to Qt "Analog Clock" under Examples/gui/analogclock/, we can add timer in Qt Control.

```cpp
  Timer {
        interval: 100; running: true; repeat: true;
        onTriggered: clock.timeChanged()
    }
```

**interval：millisecond**