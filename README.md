# LivePhotoPreviewFOVIssue

This demo is built upon Apple's official demo: "AVCam: Building a camera app" to demonstrate the issue introduced since iOS 18.3:

> When enabling Live Photo in the camera pipeline (with some other necessary settings), the preview image's FOV(Field of View) is different from the captured image.

## Necessary conditions to reproduce the issue in the camera demo app

1. Running the app on `iOS 18.3` (or `iOS 18.3.1`). As of now, the latest version is `iOS 18.3.1`.
2. Using `AVCaptureVideoDataOutputSampleBufferDelegate` to obtain the `CVPixelBuffer` and create the `CIImage` for rendering as a preview image. (Other than using `AVCaptureVideoPreviewLayer` to directly display the preview image in the UIView hierarchy.)
3. The `isLivePhotoCaptureEnabled` of `AVCapturePhotoOutput` is set to true. Whether the final captured image is a Live Photo or not is not affecting the result.
4. The `videoZoomFactor` is set to some values other than `1.0`. For example, setting the `videoZoomFactor` of `AVCaptureDevice.systemPreferredCamera` to `1.2`.

## How the issue looks

![Snipaste_2025-02-11_23-13-06](https://github.com/user-attachments/assets/d0b9a6f6-6a44-4a91-bb3e-a7f965f9367b)

As you can see, the FOV(Field of View) of the `CIImage` obtained in the `AVCaptureVideoDataOutputSampleBufferDelegate`'s method is more narrow than the preview of `AVCaptureVideoPreviewLayer` and the final captured image, which will confuse users as the preview is unreliable to frame their photos.

## Modified files in the official demo

As mentioned above, this demo to reproduce the issue is modified from the official demo of "AVCam: Building a camera app". The modified content can be found by searching "LivePhotoPreviewFOVIssue" in the code.
