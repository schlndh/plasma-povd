This repo contains instructions for installing Plasma AUR packages patched with support for per-output virtual desktops. The MRs target Plasma 6.7 which is now in development. The AUR packages backport the changes to Plasma 6.6.

## Read before proceeding:

- See [KWin MR](https://invent.kde.org/plasma/kwin/-/merge_requests/8602) for a video, a basic description of the functionality, limitations (e.g. no X11 support), ...
- **WARNING: I'm a random guy from the internet. I'm not a part of the KDE team.** Check the PKGBUILDs, patches, ... yourself before installing them.
- The MR was not yet accepted, or even fully reviewed by KDE devs. Use it on your own risk.
- Installing these packages can mess up your system. Do not proceed unless you're confident in your ability to diagnose and fix it yourself (e.g. with broken graphical session). I don't have the capacity to help you.
    - It literally happened to me as I was testing it: I built the packages while having testing repos enabled, then I disabled the testing repos and downgraded my system. Then I installed them without rebuilding, rebooted and even SDDM was broken (some incompatibility due to the packages being built with newer Qt than I had installed). I had to boot into the terminal, rebuild and reinstall the packages to get it working.
    - It happened to me yet again when I updated to rest of plasma to 6.6.1 while keeping the AUR packages at 6.6.0 (though SDDM worked this time).
- Be careful with system updates. You should update the rest of plasma packages together with the AUR packages (i.e. wait until the AUR packages are updated to the same version).
    - Make sure that you understand why [partial upgrades](https://wiki.archlinux.org/title/System_maintenance#Partial_upgrades_are_unsupported) are an issue and [how it relates to AUR](https://wiki.archlinux.org/title/Arch_User_Repository#Updating_packages).
- The AUR packages will NOT be updated when Plasma 6.7 comes out. You will have to manually uninstall them before updating to Plasma 6.7.
- It may become impractical to update the packages with new changes from the MRs once Plasma 6.7 diverges too far from 6.6. However, keeping the already backported changes in sync with new 6.6 patch releases should be fine.
- If the MR is rejected, I will stop updating the AUR packages altogether.
- If you're using third party plugins/effects/... they may work incorrectly or be completely broken because the MR changes several public APIs.
- The per-output virtual desktops are not enabled by default. You have to turn them on by checking "Switch desktops independently for each screen" in Virtual Desktops section in settings.

## Installing

Install all of these AUR packages in this exact order:

- plasma-wayland-protocols-povd (this is only a build dependency, you can skip it if you're using yay etc).
- kwin-povd
- plasma-workspace-povd
- plasma-desktop-povd

E.g. `yay -S kwin-povd plasma-workspace-povd plasma-desktop-povd`. It takes about 9 minutes to build the packages on my 9800X3D (16 threads).

## Reporting issues

If you have the AUR packages installed, **DO NOT report any issues to upstream Plasma**. First uninstall the packages and check if the issue persists. If it does, then you can report the issue upstream. Otherwise, report it to me by opening an issue in this repo.

**DO NOT report any issues related to third-party plugins/effects/...**. Not to me and especially not to the authors. It is not their responsibility to support API changes which are not even released.

## Uninstalling / Switching back to upstream packages

Run `sudo pacman -S kwin plasma-workspace plasma-desktop plasma-x11-session` and accept the removal of the `-povd` packages. You can skip `plasma-x11-session` if you didn't have it installed before.