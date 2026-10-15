# OpenSpot — Edge AI Parking Availability System

**Computer Vision · Embedded Linux · Edge AI · IoT · Full-stack Integration**

OpenSpot is a University of Tampa **team capstone project** exploring near-real-time parking availability. The integrated system combines vehicle detection at garage entrances/exits, event processing, persistence, and a browser-based dashboard.

> **My focus (Jack Lancaster):** edge-computing hardware, camera integration, YOLOv8 inference on Raspberry Pi 5 + Hailo-8L, video streaming, solar-power/enclosure integration, and entrance/exit detection development. **This was a collaborative project:** the frontend, database, API, and other sensor work had contributions from teammates. The descriptions below explicitly distinguish those components.

## System overview

```text
Entrance / Exit
      |
Camera + Raspberry Pi 5 + Hailo-8L
      |  YOLOv8 vehicle detection / event logic
      v
Networked event ingestion ------ Auxiliary sensor / temperature telemetry
      |                                      |
      +-----------------+--------------------+
                        v
                 FastAPI endpoints
                        |
              Supabase data storage
                        |
           Next.js / React dashboard
           Parking status and trends
```

*This diagram summarizes the project architecture and integration goals; individual implementations and deployments evolved over the course of development.*

## My engineering contributions

- **Embedded inference:** worked with a Raspberry Pi 5 and Hailo-8L accelerator to run a YOLOv8-based vision pipeline at the edge.
- **Camera and video:** integrated a Raspberry Pi Camera Module 3 NoIR and experimented with GStreamer-based network streaming and event transport.
- **Hardware integration:** worked on a solar-powered design using charge control and LiFePO4 storage, wake/power scheduling, thermal considerations, and a 3D-printed enclosure.
- **Garage detection:** contributed to entrance/exit detection systems, with attention to vehicle counting and avoiding double counts.

These are descriptions of work performed during prototyping; not every subsystem is represented by publicly available code in this repository.

## Team contributions and attribution

| Subsystem | Attribution |
| --- | --- |
| Raspberry Pi camera, Hailo inference, embedded/physical integration | **Jack Lancaster** (my primary focus) |
| Entrance and exit detection hardware | **Jack Lancaster and Lailani** (shared responsibilities per team documentation) |
| Backend API and parking-status logic | **Khalil Smith**, per original team README; additional code exists in Jack's backend repository |
| Database design and integrity | **Anthony**, per original team README |
| Web dashboard and data integration | **Sayed and Dhwani**, per original team README |
| Integrated Sensors repository and project work | Team repository maintained under **Imran Ali's** GitHub account; multiple contributors |

*Responsibilities are described using the team README and my project account. Contributions can evolve across branches and prototypes; see the original repositories for commit-level attribution.*

## Software and source references

- **[OpenSpot backend](https://github.com/Jaclan427/openspot-backend)** — FastAPI event ingestion, state, and Supabase integration. This is part of the overall team system, **not a claim of sole authorship of every file**.
- **[Team Sensors repository](https://github.com/imranali17689/Sensors)** — integrated Next.js frontend, FastAPI endpoints, database scripts, temperature telemetry, and project documentation. **Teammate-developed code is linked rather than copied here without permission.**

Key backend API concepts from the existing implementations:

| Route | Purpose |
| --- | --- |
| `POST /event` | Receive an entrance/exit event and update parking state |
| `GET /status` | Retrieve current parking occupancy and available capacity |
| `POST /temperature` | Store camera/device temperature telemetry in Supabase (team Sensors implementation) |

## Selected implementation concepts

A simplified example of edge-device event communication (illustrative, **not a copy of deployment code**):

```python
import requests

def report_vehicle_event(api_url: str, direction: str) -> None:
    if direction not in {"enter", "exit"}:
        raise ValueError("direction must be enter or exit")
    response = requests.post(
        f"{api_url.rstrip('/')}/event",
        json={"direction": direction},
        timeout=5,
    )
    response.raise_for_status()
```

**Engineering considerations:** stable event identifiers, retry-safe ingestion, authentication, edge/network failures, camera placement, environmental resilience, model confidence, and double-count prevention are important for production systems.

## Demonstrations and media

Screenshots of the enclosure, prototype hardware, edge inference output, and dashboard will be added here after selection. In the meantime, the source links above contain the team's development artifacts.

## Notes

- **Collaborative academic prototype**, not a production parking-control system.
- This portfolio contains **documentation and illustrative code only** until selected source modules are reviewed for ownership, secrets, deployment configuration, and licenses.
- Public repositories should never contain access tokens, private keys, service-role credentials, or sensitive live device configuration.

---

**Author / hardware and edge-AI contributor:** Jack Lancaster  
**Project:** University of Tampa capstone — OpenSpot