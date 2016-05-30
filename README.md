# AllJoynClientLib
An Universal Windows apps (UWP) Device Client Library for various common AllJoyn devices

Supported AllJoyn devices:

- Lighting Service Framework (smart lights, like LIFX or Philips Hue using [My Hue DSB Bridge](https://github.com/dotMorten/AllJoynPhilipsHueDSB)
- [AllPlay](https://www.qualcomm.com/products/allplay/platform) media players like [Gramofon](https://gramofon.com/) and [Panasonic All](http://www.panasonic.com/uk/consumer/home-entertainment/wireless-speaker-systems.html)
- Z-Wave switches using [Microsoft's Z-Wave Device Service Bridge](https://developer.microsoft.com/en-us/windows/iot/win10/samples/zwavetutorial)



### Usage:
```csharp
  
    var manager = new AllJoynClientLib.DeviceManager();
    manager.DeviceJoined += Manager_DeviceJoined;
    manager.Start();
    
    // ...

    private async void Manager_DeviceJoined(object sender, DeviceClient device)
    {
      if(device is AllJoynClientLib.Devices.LSF.LightClient)
      {
        var client = (AllJoynClientLib.Devices.LSF.LightClient)device;
        await client.SetOnOffAsync(true);
      }
      else if (device is AllJoynClientLib.Devices.AllPlay.PlayerClient)
      {
        var client = (AllJoynClientLib.Devices.AllPlay.PlayerClient)device;
        await client.MediaPlayer.NextAsync(); //Play next track
        var list = await client.MediaPlayer.GetPlaylistAsync(); //Get playlist
        await client.Volume.SetVolumeAsync(50); //Set volume
        client.MediaPlayer.PlayStateChanged += OnPlayStateChanged;
      }
    }

    private void OnPlayStateChanged(object sender, AllJoynClientLib.Devices.AllPlay.PlayState e)
    {
        //Media play state changed (start/stop/buffering/transitioning etc)
        string currentSong = e.CurrentMedia.Title;
    }
```

See the test app for more examples
