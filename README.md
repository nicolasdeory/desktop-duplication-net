# Desktop Duplication API for .NET Core

Receive desktop frame data in real time, using the Windows 8 Desktop Duplication API.

## How to use

### Initialization
```csharp
try
{
    desktopDuplicator = new DesktopDuplicator(0);
    // Desktop Duplication API initialized
}
catch (Exception ex)
{
    MessageBox.Show("An error occurred initializing the screen capture module.\nException: \n" + ex.ToString());
}
````
### Screen Capture
```csharp
public static Bitmap GetNextFrame()
{
    try
    {
        DesktopFrame frame = desktopDuplicator.GetLatestFrame();
        if (frame != null)
        {
            Bitmap frameBitmap = frame.DesktopImage;
            return frameBitmap;

        }
    }
    catch (Exception)
    {
        desktopDuplicator.Dispose();
        desktopDuplicator = new DesktopDuplicator(0);
        // Don't worry, exceptions are expected to happen
    }
    return null;
}
```
`DesktopDuplicationException` exceptions can occur when there is a change in the display mode, or a new frame isn't available yet. This can happen as a result of switching to a full-screen app, for example.
The `DesktopDuplicator` object needs to be reinitialized because the SharpDX output duplication must be created for the specific display mode.
You must call `Dispose` on the `DesktopDuplicator` object before creating a new one, or you will have memory leaks.
