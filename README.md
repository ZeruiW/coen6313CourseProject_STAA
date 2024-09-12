# Video Attention Analysis Service

This service provides video attention analysis using a TimesFormer model. It processes uploaded videos, generates attention heatmaps, predicts actions based on the Kinetics-400 dataset, and provides various visualization outputs.

## Features

- Video upload and processing

- Attention heatmap generation

- Action prediction using Kinetics-400 dataset

- JSON data output for frame-by-frame attention analysis

- Temporal saliency visualization

## Datasets

The video clip dataset can be found in Kaggle. Kinetics-400 is a benchmark dataset for video models.

  https://www.kaggle.com/datasets/ipythonx/k4testset

## API Endpoints

 Base Url for the video XAI service: `http://xaiport.ddns.net:6313` 

The service exposes the following RESTful API endpoints:

### 1. Upload Video

- **URL**: `/upload_video/`
- **Method**: POST
- **Content-Type**: multipart/form-data
- **Body**: 
  - `file`: The video file to be uploaded (MP4 format recommended)
- **Response**: 
  ```json
  {
    "video_id": "unique_video_identifier",
    "message": "Video uploaded and processing started"
  }
  ```

### 2. Check Video Processing Status

- **URL**: `/video_status/{video_id}`
- **Method**: GET
- **Response**: 
  ```json
  {
    "status": "processing|completed|failed",
    "error": "Error message if failed",
    "predicted_label": 42,
    "predicted_action": "juggling balls",
    "heatmap_video_url": "/download/{video_id}/heatmap_video",
    "json_data_url": "/download/{video_id}/json_data",
    "visualization_url": "/download/{video_id}/visualization"
  }
  ```

### 3. Download Results

- **URL**: `/download/{video_id}/{file_type}`
- **Method**: GET
- **file_type options**: 
  - `heatmap_video`: Attention heatmap video
  - `json_data`: JSON file containing frame-by-frame attention data
  - `visualization`: PNG image of temporal saliency visualization
- **Response**: The requested file

## Usage Example

Here's a general workflow for using the API:

1. Upload a video:
   - Send a POST request to `/upload_video/` with the video file.
   - Receive a `video_id` in response.

2. Check processing status:
   - Periodically send GET requests to `/video_status/{video_id}`.
   - Continue checking until the status is "completed" or "failed".
   - When completed, you'll receive the predicted action and label.

3. Download results:
   - Once processing is complete, use the URLs provided in the status response to download the results.

## API Usage with cURL

Here are examples of how to use the API with cURL:

1. Upload a video:
   ```
   curl -X POST -F "file=@/path/to/your/video.mp4" http://xaiport.ddns.net:6313/upload_video/
   ```

2. Check video status:
   ```
   curl http://xaiport.ddns.net:6313/video_status/{video_id}
   ```

3. Download results:
   ```
   curl -O http://xaiport.ddns.net:6313/download/{video_id}/heatmap_video
   curl -O http://xaiport.ddns.net:6313/download/{video_id}/json_data
   curl -O http://xaiport.ddns.net:6313/download/{video_id}/visualization
   ```

Replace `{video_id}` with the actual video ID received from the upload response.

## Gradio Interface

A Gradio-based web interface is available for easy interaction with the service. To use it:

1. Ensure the main service is running.
2. Run the Gradio application script.
3. Open the provided URL in your web browser.
4. Upload a video and view the processing results, including the predicted action.

## Notes

- The server runs on `http://xaiport.ddns.net:6313` by default. Adjust the URL if your server is hosted differently.
- Ensure your video is in a format supported by the service (MP4 recommended).
- Processing time may vary depending on the length and complexity of the video.
- Always check the processing status before attempting to download results.

For any issues or further questions, please contact the service administrator.
