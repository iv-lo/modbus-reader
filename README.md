# Modbus Data Logger – Configuration Guide

This application connects to a Modbus TCP server, reads input registers from specified sensors, and logs the values to a CSV file.

## 🔧 Configuration: `config.json`

The application uses an external `config.json` file to define connection settings and sensor metadata.

### ✅ Required Fields:

```json
{
  "modbus_host": "169.254.174.249",
  "modbus_port": 502,
  "poll_interval_sec": 5,
  "registers": {
    "SENSOR_NAME": {
      "address": REGISTER_ADDRESS,
      "description": "Human-readable description"
    }
  }
}
```

### 📝 Field Descriptions:

| Field               | Type     | Description                                                                 |
|---------------------|----------|-----------------------------------------------------------------------------|
| `modbus_host`       | string   | IP address or hostname of the Modbus TCP server.                           |
| `modbus_port`       | integer  | TCP port of the Modbus server (default is `502`).                          |
| `poll_interval_sec` | integer  | Interval between Modbus polling requests (in seconds).                     |
| `registers`         | object   | A dictionary of sensor tags. Keys are sensor names, values contain:        |
| → `address`         | integer  | Modbus register address (input register, zero-based: e.g. `30030` → `30`). |
| → `description`     | string   | Optional description (used for clarity, not used in code logic).           |

---

### 📌 Example:

```json
{
  "modbus_host": "169.254.174.249",
  "modbus_port": 502,
  "poll_interval_sec": 5,
  "registers": {
    "PT101_Rosemount_Bottom": {
      "address": 30,
      "description": "Rosemount Bottom"
    },
    "PT102_WIKA_Top": {
      "address": 13,
      "description": "WIKA P31 Top of Tank"
    }
  }
}
```

---

### 📂 Output

- Collected data is saved to `modbus_data_log.csv` in the same directory as the executable.
- Each row contains a timestamp and the corresponding sensor values.

---

### ⚠️ Modbus Address Notes

Modbus input register addresses often appear in the form `30001`, `30009`, etc.
However, in the configuration file, you should provide the **zero-based offset**:

- `30001` → `1`
- `30009` → `9`
- `30030` → `30`

This is because the underlying Modbus protocol uses address offsets, not full register numbers.

👉 Do **not** include the `30000` prefix in the `address` field.