---
date : 2023-05-11 15:20
priority : 1
---
# Metadata
Status ::
Type ::
# Question
How to write a code to debug  original code?
# Answer
# Note
1. using a new thread to open gdb
2. using system to make sure original thread is tracer by gdb and wait for moment to ensure gdb working
3. in the end debug contain, doing thread wait for gdb to quit
```cpp
  const pid_t pid = getpid();

  // std::future<void> gdbtask = std::async(std::launch::async, 
  std::thread gdbthread
    ( [&]() {
      if (!debug_weight) return;
      std::string cmd = "xterm -fa 'Monospace' -fs 14 -T \"Debug CF Weight\" -e gdb -p " + std::to_string(pid);
      tout << "Starting gdb...: " << cmd << std::endl;

      if (-1 == system(cmd.c_str())) // invoke gdb and wait to finish in this thread
        logError() << "gdb" << std::endl;
      debug_weight = false;
      tout << "GDB terminated." << std::endl;
      });

  if (debug_weight) {
    const std::string statFileName = "/proc/" + std::to_string(pid) + "/status";
    std::ifstream statFile(statFileName);
    std::string word;
    int tracerPid = 0;
 
    // wait for GDB started or error
    while (debug_weight) {
      statFile.seekg(0);
      while (statFile >> word) {
        if (word == "TracerPid:") { // check the tracerPid 
          statFile >> tracerPid;
          break;
        }
      }

      if (tracerPid) {
        tout << "Process is attached by GDB." << endl;
        break;
      }
      sleep(1); // wait for gdb ready
    }
  }
  ... // portion to debug
  gdbthread.join(); // wait gdb stop and continue work
}
```