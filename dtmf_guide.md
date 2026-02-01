# DTMF & IVR Navigation Guide

This guide explains how the voice agent interacts with automated phone menus (IVR) using DTMF tones (keypad digits) and how it detects keypad presses from the user.

## 1. Sending DTMF (IVR Navigation)

The agent uses the `send_dtmf_code` tool to navigate automated menus.

### Function: `send_dtmf_code`
*   **Location**: `agent.py` -> `CallTools` class.
*   **Purpose**: Sends a specific keypad digit (0-9, *, #) to the phone system.
*   **Mechanism**: Uses `ctx.room.local_participant.publish_dtmf`.
*   **Cooldown**: Includes a 3-second cooldown to prevent sending tones too rapidly, ensuring the IVR system processes them correctly.

### How to use:
Simply ask the agent to press a key.
- *"Press 1 for sales."*
- *"Navigate the menu and select option 2."*
- *"Press hash to confirm."*

---

## 2. Receiving DTMF (Keypad Detection)

The agent can "hear" when the caller presses keys on their phone keypad.

### Function: `on_dtmf_received` (Event Listener)
*   **Location**: `agent.py` -> `entrypoint` function.
*   **Purpose**: Detects digits pressed by the user during the call.
*   **Mechanism**: Listens for the `sip_dtmf_received` event on the LiveKit room.
*   **Behavior**:
    - Logs the keypress in the terminal.
    - Triggers a specific AI response (e.g., acknowledging the selection).

### Custom Logic:
You can customize the responses in `agent.py`:
```python
@ctx.room.on("sip_dtmf_received")
def on_dtmf_received(dtmf: rtc.SipDTMF):
    if dtmf.digit == "1":
        # Specific logic for key 1
        session.generate_reply(instructions="User pressed 1. Thank them for selecting option 1.")
```

---

## 3. Why use this?
- **Automated Outreach**: Use the agent to call businesses and navigate their menus automatically.
- **Interactive Callbacks**: Allow users to provide input (like confirming an appointment) via their keypad instead of speaking.
