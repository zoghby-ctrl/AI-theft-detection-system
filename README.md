# AI-theft-detection-system
AI-Powered IoT Package Theft Detection System
This project is a multi-sensor, AI-enabled IoT system designed to detect and deter package theft. It's built around the ESP32-CAM, which provides camera, WiFi, and AI processing capabilities in a single, low-cost module.

The system's logic uses a multi-sensor approach to intelligently detect threats:

A PIR sensor detects motion (like a person approaching).

An Ultrasonic sensor verifies that a package is present in the "safe zone."

A Vibration/Tilt sensor detects tampering or if the package is being moved.

If a threat is identified (e.g., motion is detected while a package is present, or the package is tampered with), the system activates a local alarm (loud buzzer and LED) to deter the thief. Simultaneously, it uses its IoT capabilities to send a real-time notification to the user, with the AI model helping to verify the threat.
