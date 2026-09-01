# Temperature Monitor - Capstone Project


## 📖 Here is how I built this peoject

### Step 1️⃣ 

I did this - set up my Ubuntu VM and installed all the build tools:

![Step 1: Got all tools installed](screenshots/Screenshot%202026-09-01%20125330.png)

---

### Step 2️⃣ - Go to Project Folder

Downloaded and extracted the project folder:

```bash
cd capstone-temp-monitor
ls -la
```

I organized it like this:
- `driver/` - the kernel code
- `app/` - the C++ program  
- `README.md` - instructions

![Step 2: Project structure all set](screenshots/Screenshot%202026-09-01%20125625.png)

---

### Step 3️⃣ - Build the Kernel Driver

```bash
cd driver
make
```

This compiles the driver and makes `tempsensor.ko` file. I did this:

![Step 3: Building the kernel module](screenshots/Screenshot%202026-09-01%20130259.png)

If there are build errors, try:
```bash
make clean
make
```

I verified it worked:

![Step 3: Build successful](screenshots/Screenshot%202026-09-01%20131539.png)

---

### Step 4️⃣ - Load the Driver into Linux

```bash
sudo insmod tempsensor.ko
dmesg | tail -5
```

This loads the driver. I did this - you'll see it loaded:

![Step 4: Driver loaded into kernel](screenshots/Screenshot%202026-09-01%20131927.png)

Check if device file shows up:

```bash
ls -l /dev/tempsensor
```

I confirmed the device file is there:

![Step 4: Device file created](screenshots/Screenshot%202026-09-01%20131609.png)

---

### Step 5️⃣ - Test the Driver (Quick Check)

Read temperature directly from the device:

```bash
sudo cat /dev/tempsensor
sudo cat /dev/tempsensor
sudo cat /dev/tempsensor
```

Run it a few times. Temperature changes each time because it's simulating a real sensor:

![Step 5: Reading temperatures from driver](screenshots/Screenshot%202026-09-01%20132023.png)

I read it multiple times and numbers drifted like a real sensor:

![Step 5: Temperatures changing slightly](screenshots/Screenshot%202026-09-01%20132039.png)

---

### Step 6️⃣ - Build the C++ App

```bash
cd ../app
g++ -Wall -O2 -o temp_monitor temp_monitor.cpp
```

This makes the app that reads from the driver and watches the state:

![Step 6: Built the C++ app](screenshots/Screenshot%202026-09-01%20132138.png)

---

### Step 7️⃣ - Run the Monitor App

```bash
sudo ./temp_monitor
```


I ran it and watched the state machine work:

![Step 7: App running, showing temperatures](screenshots/Screenshot%202026-09-01%20133421.png)

Wait a bit more... and we see the state change:

![Step 7: Temperature rose, state changed to WARNING](screenshots/Screenshot%202026-09-01%20133537.png)


---

### Step 8️⃣ - Speed Up the Demo (Optional)

Want to see CRITICAL state fast? Use drift mode:

```bash
sudo ./temp_monitor --drift 40
```

This makes temperature rise super fast. I did this to show all states quickly:

![Step 8: Fast demo mode showing all transitions](screenshots/Screenshot%202026-09-01%20133636.png)

---

### Step 9️⃣ - Cleanup

When done, unload the driver:

```bash
sudo rmmod tempsensor
```

Done! Everything cleaned up.

---

## 📁 What's Inside

```
driver/               ← Kernel code
  tempsensor.c       (the fake sensor)
  tempsensor_ioctl.h (how to talk to it)
  Makefile           (build instructions)

app/                 ← User program
  temp_monitor.cpp   (watches the temperature)

README.md            (more detailed guide)
LICENSE              (MIT license)
.gitignore           (tells git what to ignore)
screenshots/         (these pictures!)
```

---



