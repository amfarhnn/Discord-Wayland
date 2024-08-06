The error you're encountering indicates that several packages depend on a `pipewire-session-manager` to function properly. Both `wireplumber` and `pipewire-media-session` can serve as session managers for PipeWire, but your system currently has `wireplumber` installed as the session manager. To resolve this dependency issue and switch to `pipewire-media-session`, you will need to replace `wireplumber` with `pipewire-media-session` without breaking the dependencies.

Here's how you can proceed:

### Steps to Replace `wireplumber` with `pipewire-media-session`

1. **Use the `pacman` `-d` Option:**

   You can use the `--asdeps` option to remove packages marked as dependencies and install `pipewire-media-session`. However, it's essential to ensure that you are only replacing the session manager, not removing PipeWire itself.

2. **Remove `wireplumber` and Install `pipewire-media-session`:**

   Use the following command to replace `wireplumber` with `pipewire-media-session`:

   ```bash
   sudo pacman -S pipewire-media-session --asdeps
   ```

   This command will replace `wireplumber` with `pipewire-media-session`, ensuring that all dependencies are satisfied.

3. **Restart PipeWire Services:**

   After making these changes, restart the PipeWire services to apply the changes:

   ```bash
   systemctl --user restart pipewire pipewire-pulse
   ```

4. **Check the Status of Services:**

   Verify that the PipeWire services are running correctly with the new session manager:

   ```bash
   systemctl --user status pipewire
   systemctl --user status pipewire-pulse
   systemctl --user status pipewire-media-session
   ```

### Optional: Reinstall Dependencies

If you encounter any issues with the above steps, you may need to temporarily remove dependent packages, switch the session manager, and then reinstall the dependencies:

1. **Remove Conflicting Packages:**

   Temporarily remove the conflicting packages:

   ```bash
   sudo pacman -Rns obs-pipewire-audio-capture gst-plugin-pipewire pipewire-alsa pipewire-jack pipewire-pulse xdg-desktop-portal-wlr
   ```

2. **Install `pipewire-media-session`:**

   Install the new session manager:

   ```bash
   sudo pacman -S pipewire-media-session
   ```

3. **Reinstall Dependencies:**

   Reinstall the previously removed packages:

   ```bash
   sudo pacman -S gst-plugin-pipewire pipewire-alsa pipewire-jack pipewire-pulse xdg-desktop-portal-wlr
   ```

4. **Restart Services:**

   Restart the services as mentioned earlier:

   ```bash
   systemctl --user restart pipewire pipewire-pulse
   ```

### Summary

Switching from `wireplumber` to `pipewire-media-session` requires careful handling of dependencies, ensuring that no critical packages are removed in the process. By using the `--asdeps` flag, you can replace the session manager without disrupting the rest of the system.

If you still encounter issues or need further assistance, please let me know!


The error message you encountered indicates that Discord is unable to find or access video sources for screen sharing. This can happen if the integration between PipeWire and Discord isn’t set up correctly. Let's go through some troubleshooting steps to resolve this issue:

### Steps to Fix Screen Sharing with Discord on Wayland

1. **Verify PipeWire and Portal Services:**

   Ensure that all necessary services are running correctly. The `xdg-desktop-portal` service is crucial for screen sharing on Wayland.

   ```bash
   systemctl --user status pipewire
   systemctl --user status pipewire-pulse
   systemctl --user status pipewire-media-session
   systemctl --user status xdg-desktop-portal
   systemctl --user status xdg-desktop-portal-wlr
   ```

   If any service is not running, try starting it:

   ```bash
   systemctl --user start pipewire
   systemctl --user start pipewire-pulse
   systemctl --user start pipewire-media-session
   systemctl --user start xdg-desktop-portal
   systemctl --user start xdg-desktop-portal-wlr
   ```

2. **Check Your Compositor Configuration:**

   Ensure that your compositor is configured correctly to work with `xdg-desktop-portal-wlr`. For Sway, this means you need to start Sway in a D-Bus session:

   - Ensure your Sway configuration (`~/.config/sway/config`) includes:

     ```bash
     exec dbus-run-session -- sway
     ```

   - If you already have a running session, try restarting Sway to ensure it is running under D-Bus:

     ```bash
     swaymsg exit
     dbus-run-session -- sway
     ```

3. **Check Discord Launch Command:**

   Make sure you are launching Discord with the correct command to use Wayland:

   - Edit your custom Discord launch script or directly launch it using:

     ```bash
     electron --enable-features=UseOzonePlatform --ozone-platform=wayland /opt/discord/resources/app.asar
     ```

   - Ensure this script is executable:

     ```bash
     chmod +x ~/bin/discord-wayland
     ```

4. **Verify PipeWire Configuration:**

   Ensure your PipeWire configuration supports video streams. You may need to ensure `pipewire` and `xdg-desktop-portal` configurations are set correctly:

   - Open `~/.config/pipewire/pipewire.conf` and ensure it’s configured correctly (default settings usually work fine).

   - Verify `xdg-desktop-portal` settings at `~/.config/xdg-desktop-portal/portals.conf` if necessary.

5. **Check for Application Permissions:**

   Ensure that the required permissions are granted for screen sharing:

   - Check your Wayland compositor settings to ensure it allows Discord to access screen sharing features.

   - Sometimes, security policies might block these actions, so review any relevant configurations.

6. **Check for Updated Packages:**

   Ensure all your packages are up-to-date, especially PipeWire, portals, and Discord:

   ```bash
   sudo pacman -Syu
   ```

   - If you’ve installed Discord from AUR, ensure it’s the latest version using your AUR helper (`yay`, `paru`, etc.).

7. **Restart the System:**

   Sometimes restarting your machine can ensure all services start correctly and communicate with each other:

   ```bash
   sudo reboot
   ```

### Additional Debugging

If the above steps do not resolve the issue, you can perform further debugging:

- **Check Logs:**

  Look at logs for `pipewire`, `xdg-desktop-portal`, and `discord` to get more insights:

  ```bash
  journalctl --user -xe --unit=pipewire
  journalctl --user -xe --unit=xdg-desktop-portal
  ```

- **Manually Launch Discord with Debug Logs:**

  You can start Discord from the terminal to see detailed error messages:

  ```bash
  electron --enable-features=UseOzonePlatform --ozone-platform=wayland /opt/discord/resources/app.asar --log-level=debug
  ```

This will help you identify more specific issues in the integration process.

By following these steps, you should be able to resolve the screen sharing issue on Discord with Wayland and PipeWire. If you need any further assistance or run into more issues, feel free to ask!

The error messages you're seeing in the logs indicate that there is an issue with `xdg-desktop-portal-wlr` and its connection to PipeWire. The service is inactive, which means it might not be starting correctly, or it's failing due to configuration issues. Let's go through some steps to troubleshoot and fix this issue:

### Steps to Fix `xdg-desktop-portal-wlr` and PipeWire Issues

1. **Start the `xdg-desktop-portal-wlr` Service Manually:**

   Try starting the `xdg-desktop-portal-wlr` service manually to see if it produces any errors or logs that can help diagnose the problem:

   ```bash
   systemctl --user start xdg-desktop-portal-wlr
   ```

   Check its status after starting:

   ```bash
   systemctl --user status xdg-desktop-portal-wlr
   ```

   If it fails to start, check for any error messages in the output that can provide more information.

2. **Enable the Service at Startup:**

   Make sure the service is enabled to start at boot:

   ```bash
   systemctl --user enable xdg-desktop-portal-wlr
   ```

3. **Check Service Logs:**

   Check the logs for `xdg-desktop-portal-wlr` to see if there are any specific errors:

   ```bash
   journalctl --user -xe --unit=xdg-desktop-portal-wlr
   ```

   Look for any error messages that might indicate what is going wrong.

4. **Verify PipeWire Configuration:**

   Ensure that PipeWire is correctly configured and running:

   ```bash
   systemctl --user restart pipewire pipewire-pulse
   ```

   Check the status of these services:

   ```bash
   systemctl --user status pipewire
   systemctl --user status pipewire-pulse
   ```

5. **Ensure D-Bus is Running:**

   Since `xdg-desktop-portal-wlr` relies on D-Bus for communication, make sure that your session has D-Bus running:

   - Verify that `dbus` is running in your session:

     ```bash
     echo $DBUS_SESSION_BUS_ADDRESS
     ```

   - If it's not set, you can start a D-Bus session manually:

     ```bash
     dbus-launch --exit-with-session
     ```

6. **Check Wayland Compositor Configuration:**

   Make sure that your Wayland compositor (e.g., Sway) is configured to support screen sharing through PipeWire:

   - Ensure that `~/.config/sway/config` or equivalent for your compositor includes the following line:

     ```bash
     exec dbus-run-session -- sway
     ```

   - Restart your compositor to apply the changes:

     ```bash
     swaymsg exit
     dbus-run-session -- sway
     ```

7. **Update Packages:**

   Ensure all your packages, including PipeWire and `xdg-desktop-portal-wlr`, are up-to-date:

   ```bash
   sudo pacman -Syu
   ```

8. **Check for Conflicting Software:**

   Sometimes, other software might interfere with the portals or PipeWire. Ensure that no other screen-sharing services are running that might conflict.

9. **Alternative Portals:**

   If `xdg-desktop-portal-wlr` continues to cause issues, consider trying a different portal implementation, like `xdg-desktop-portal-gtk`, which can serve as a fallback:

   ```bash
   sudo pacman -S xdg-desktop-portal-gtk
   ```

   You can specify which portal to use by setting the environment variable `XDG_CURRENT_DESKTOP=KDE` or `XDG_CURRENT_DESKTOP=GNOME`, but these are usually configured automatically.

### Testing Screen Sharing

After following these steps, test screen sharing in Discord again:

1. **Start Discord with Wayland:**

   Make sure you're starting Discord with Wayland support using your custom script or directly:

   ```bash
   electron --enable-features=UseOzonePlatform --ozone-platform=wayland /opt/discord/resources/app.asar
   ```

2. **Attempt Screen Sharing:**

   - Initiate a screen-sharing session to verify functionality.
   - If it still doesn't work, check the logs again for any additional error messages.

By following these steps, you should be able to resolve the issue with `xdg-desktop-portal-wlr` and enable screen sharing on Wayland with Discord. If you encounter more issues or need further assistance, feel free to ask!


Use qpwgraph
