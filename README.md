# Top-Down Shooter (UE 5.6)

![](https://img.shields.io/badge/Unreal_Engine-5.6-313131?style=for-the-badge&logo=unrealengine&logoColor=white)
![](https://img.shields.io/badge/Language-Blueprints-0052CC?style=for-the-badge&logo=unrealengine)
![](https://img.shields.io/badge/Input-Enhanced_Input_System-green?style=for-the-badge)
![](https://img.shields.io/badge/Status-Learning_Project-orange?style=for-the-badge)

A top-down shooter base built in Unreal Engine 5.6 as a primary learning project. The focus was to establish solid core gameplay mechanics from scratch, including camera-relative movement, Enhanced Input actions, and accurate cursor tracking using mathematical line-plane intersections.

---

## Technical Features

![](https://img.shields.io/badge/Feature-Camera--Relative_Movement-007ACC?style=flat-square)  
Standard directional WASD movement configured relative to the overhead camera perspective rather than character facing direction, implemented via Unreal's Enhanced Input framework (`INP_Move`).

![](https://img.shields.io/badge/Feature-Line--Plane_Intersection_Aiming-007ACC?style=flat-square)  
Replaced traditional cursor line traces with a deprojected screen-to-world ray intersected against an infinite horizontal plane (`Z = 1.0` normal). This prevents aiming lockups or snapping when the cursor moves past floor bounds or touches wall collision.

![](https://img.shields.io/badge/Feature-Decoupled_SpringArm-007ACC?style=flat-square)  
Configured the `SpringArm` component with `Use Pawn Control Rotation` and `Inherit Yaw` disabled. The camera stays fixed overhead while the player character rotates independently across 360 degrees.

![](https://img.shields.io/badge/Feature-Twin--Stick_%26_Mouse_Support-007ACC?style=flat-square)  
Built dual input support for both mouse cursor orientation (`INP_LookAround`) and gamepad right-thumbstick directional aiming (`INP_AutoFire`).

![](https://img.shields.io/badge/Feature-2D_BlendSpace_Integration-007ACC?style=flat-square)  
Movement animations blend smoothly across directional axes based on character velocity vectors.

---

## Line-Plane Intersection Aiming

Instead of raycasting against world static geometry (which fails when pointing off the floor mesh or onto vertical walls), the system projects a ray from screen coordinates onto a flat plane:

```text
[Get Mouse Position]
        │
        ▼
[Deproject Screen to World]
        │
        ▼
[Line Plane Intersection] ── Plane Normal: (0, 0, 1.0) | Plane Origin: Player Location Z
        │
        ▼
[Find Look at Rotation] ──── Extract Yaw Axis
        │
        ▼
[Set Control Rotation]
```

1. **Deproject Screen to World**: Translates 2D screen cursor coordinates into a 3D world location and direction vector.
2. **Line Plane Intersection**: Calculates the intersection point of the 3D ray on an infinite plane at the player's Z-height (`Plane Normal Z = 1.0`).
3. **Yaw Isolation**: Passes the target world position into `Find Look at Rotation` and extracts only the **Yaw** axis, keeping Pitch and Roll locked at `0.0` so the character stays level.

---

## Controls

| Action | Keyboard / Mouse | Gamepad |
| :--- | :--- | :--- |
| **Move** | `W`, `A`, `S`, `D` | Left Thumbstick |
| **Aim** | Mouse Cursor | Right Thumbstick |
| **Fire** | Left Mouse Button | Right Trigger (`RT`) |

---

## Setup & Running

1. Clone or download this repository.
2. Open `TopDownShooter.uproject` in **Unreal Engine 5.6**.
3. Load `MainMap` from the Content Browser.
4. Press **Play in Editor (PIE)**.