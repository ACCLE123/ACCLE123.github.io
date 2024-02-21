## 1. 列出所有摄像头

- **端点：** `GET /system/camera`
- **描述：** 获取有关系统中所有摄像头的信息。
- **响应格式：** JSON
- **示例响应：**
    ```json
    [
        {
            "camera_id": 1,
            "name": "摄像头 1",
            "url": "http://camera1.com",
            "my_url": "http://mycamera1.com"
        },
        {
            "camera_id": 2,
            "name": "摄像头 2",
            "url": "http://camera2.com",
            "my_url": "http://mycamera2.com"
        }
    ]
    ```

### 2. 根据 ID 获取摄像头
- **端点：** `GET /system/camera/<int:cameraId>`
- **描述：** 根据摄像头的 ID 获取有关特定摄像头的信息。
- **参数：**
    - `cameraId`（int）：摄像头的 ID。
- **响应格式：** JSON
- **示例响应：**
    ```json
    [
        {
            "camera_id": 1,
            "name": "摄像头 1",
            "url": "http://camera1.com",
            "my_url": "http://mycamera1.com"
        }
    ]
    ```

### 3. 添加新摄像头
- **端点：** `POST /system/camera`
- **描述：** 向系统添加新摄像头。
- **请求格式：** JSON
- **请求体：**
    ```json
    {
        "name": "新摄像头",
        "url": "http://newcamera.com",
        "my_url": "http://mynewcamera.com"
    }
    ```
- **响应格式：** 普通文本
- **示例响应：** `success`
