## The problem space
Windows as Windows 11 22H2 doesn't have any support for foldable PCs. Amongst many things, the OS is lacking APIs to :

* Access the current posture of the device.
* Establish the current screen topolgy, for example the fold size and fold position.

PC manufacturers have identified some of the shortcomings of Windows and most of them have created some type of support to improve the foldable experience on Windows. For example [Asus](https://www.asus.com/) have a software preinstalled [ScreenXpert](https://apps.microsoft.com/store/detail/screenxpert/9N5RFFGFHHP6?hl=en-us&gl=us&rtc=1) which is intended to help by, for example, adding Window Manager capabilities to make the form factor better. [Lenovo](https://lenovo.com/) has a similar software called [Mode Switcher](https://support.lenovo.com/us/en/downloads/ds546903-lenovo-mode-switcher-for-windows-10-64-bit-thinkpad-x1-fold-gen-1-types-20rk-20lk). These software comes preinstalled with the laptop to provide a good user experience.

## Windows has an Hinge Angle API
It's true that Windows has an [hinge angle API](https://learn.microsoft.com/en-us/uwp/api/windows.devices.sensors.hingeanglesensor?view=winrt-22621) which could be used to derive the posture of the laptop. However this API is not enough because the hinge angle is not the only data used to compute the posture, for example if the keyboard is docked or not is an important aspect to determine the posture. The status of the kickstand (deployed or not) could also be used to determine the posture.

# Proposal
Because most foldable computer manufacturers ship some type of software to fill the gap of Windows we can try to work with them to find a way to expose the data that we need to support foldables on the web.

We have two APIs for the web targeting foldables :
* [The Device Posture API](https://www.w3.org/TR/device-posture/)
* [The Viewport Segments API](https://github.com/WICG/visual-viewport/blob/gh-pages/segments-explainer/SEGMENTS-EXPLAINER.md)

Because OEMs build amnd distribute their software differently it will be complicated to come up with a SDK that would work for all. Furthermore adding an external dependency may not be suitable for 3rd party apps wanting to use these information.

Taking inspiration from existing APIs, Windows provide a registry entry to determine if the device is being used at a tablet or with a keyboard (for example Surface Pro devices). The API is documented [here](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-gpiobuttons-convertibleslatemode). Chromium based browsers use [this API](https://source.chromium.org/chromium/chromium/src/+/main:ui/events/devices/input_device_observer_win.cc;l=70?q=ConvertibleSlateMode&sq=) to resolve the proper primary input type and it's the recommended way to do so. Mozilla also uses this API in Firefox.

# Long term
We hope that this work is temporary, that ultimately Microsoft will propose APIs directly in Windows to address these use cases. In this case this will be deprecated as soon as possible
