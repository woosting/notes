# Pinning

Prefer certain packages (from section components) over others during system installs and/or upgrades:


## Pinning from non-free

Install one proprietary package from the non-free section component:

1.	Open the repository listing file for editing:

    ```
    # vim /etc/sources.list
    ```

2.	Append the relevant repository lines with: `contrib non-free`:

    ```shell
    deb http://ftp.us.debian.org/debian stable main contrib non-free
    ```

3.	Open (or create) the preferences file for editing:

	```
	# vim /etc/apt/preferences
    ```

4.	Append it with your pinning preferences:

    ```shell
    # Disable PROPRIETARY packages by default
    Package: *
    Pin: release a=stable,c=non-free
    Pin-Priority: -1

    # Disable PROPRIETARY-DEPENDENT packages by default
    Package: *
    Pin: release a=stable,c=contrib
    Pin-Priority: -1

    # ---

    # Enable SPECIFIC PROPRIETARY package (e.g. WiFi firmware in this case)
    Package: firmware-iwlwifi
    Pin: release a=stable,c=non-free
    Pin-Priority: 600
    ```

    >	With: High [pin priorities][3] prevailing over the lower (see: [apt_preferences man page][5]):
    >
    >	```
    >	Priorities (P) assigned in the APT preferences file must be positive or negative integers. They are interpreted as follows (roughly speaking):
    >
    >	P >= 1000
    >	causes a version to be installed even if this constitutes a downgrade of the package
    >
    >	990 <= P < 1000
    >	causes a version to be installed even if it does not come from the target release, unless the installed version is more recent
    >
    >	500 <= P < 990
    >	causes a version to be installed unless there is a version available belonging to the target release or the installed version is more recent
    >
    >	100 <= P < 500
    >	causes a version to be installed unless there is a version available belonging to some other distribution or the installed version is more recent
    >
    >	0 < P < 100
    >	causes a version to be installed only if there is no installed version of the package
    >
    >	P < 0
    >	prevents the version from being installed
    >	```


### Example case

---

_!!! WARNING !!! -- This resulted in a broken X on a LENOVO T460 -- !!! WARNING !!!_

---

Install Debian's `firmware-iwlwifi` (Intel propriety firmware) package and the kernal-image packages from the stretch (testing at the moment of writing) repository, while suppressing all other packages from non-free and stretch:

1. Perform the aforementioned and add the following to the `/etc/apt/preferences` file, to add the specific references to the correspondingly specific packages from the correspondingly specific repositories:

	```shell
	# === STABLE ===

	# Enable all packages from STABLE main tree by default,
	# with a high priority to make it the preferred source:
	Package: *
	Pin: release a=stable,c=main
	Pin-Priority: 900

	# ---

	# Disable packages from STABLE non-free tree by default:
	Package: *
	Pin: release a=stable,c=non-free
	Pin-Priority: -1

	# Disable packages from STABLE contrib tree by default:
	Package: *
	Pin: release a=stable,c=contrib
	Pin-Priority: -1


	# === STRETCH ===

	# Disable packages from STRETCH by default:
	Package: *
	Pin: release n=stretch
	Pin-Priority: -1

	# ---

	# Enable WIFI FIRMWARE package from STRETCH non-free tree
	Package: firmware-iwlwifi
	Pin: release n=stretch,c=non-free
	Pin-Priority: 600

	# Enable KERNEL package from STRETCH main tree
	Package: linux-image*
	Pin: release n=stretch,c=main
	Pin-Priority: 600
	```

2. Install the two packages with:

	```
	# apt-get install firmware-iwlwifi/stretch linux-image-4.9.0-1-amd64/stretch
	```

## References:

> Adapted from: Debian Wiki
> [AptPreferences][1]

> Adapted from: Debian (obsolete) docs - Apt how-to
> [Chapter 3 - Managing packages][2]

> Adapted from: HowtoForge
> [A Short Introduction To Apt-Pinning][3]

> Adapted from: (ServerFault)
> [How do I enable non-free packages on Debian?][4]


<!-- REFERENCES -->

[1]:https://wiki.debian.org/AptPreferences
[2]:https://www.debian.org/doc/manuals/apt-howto/ch-apt-get.en.html
[3]:https://www.howtoforge.com/a-short-introduction-to-apt-pinning
[4]:http://serverfault.com/a/580700/372187
[5]:https://
