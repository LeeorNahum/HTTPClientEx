# HTTPClientEx

HTTPClientEx is an HTTP client for Arduino-ESP32 with extended 32-bit read timeouts. It helps when servers (e.g., AI APIs) take longer than 65 seconds to send data.

## Installation

### Arduino IDE

1. Download this library as a ZIP.
2. Arduino IDE → `Sketch` → `Include Library` → `Add .ZIP Library...`.

### PlatformIO

Add to `platformio.ini`:

```ini
lib_deps =
  https://github.com/LeeorNahum/HTTPClientEx.git
```

## Usage

```cpp
#include <WiFiClientSecure.h>
#include <HTTPClientEx.h>

void setup() {
  Serial.begin(115200);
  WiFiClientSecure client;
  client.setInsecure();

  HTTPClientEx http;
  http.begin(client, "https://example.com/slow" );
  http.setTimeout((uint32_t)180000); // 3 minutes
  int code = http.GET();
  if (code > 0) {
    Serial.println(http.getString());
  }
  http.end();
}

void loop() {}
```

## Why

The stock `HTTPClient` uses a 16-bit read timeout, effectively limiting waits to ~65.5 seconds. `HTTPClientEx` uses a 32-bit timeout and synchronizes it with the underlying socket and Stream layers.
