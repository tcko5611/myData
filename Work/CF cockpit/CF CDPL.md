---
date : 2023-03-10 10:43
priority : 1
---
# Metadata
Status ::
Type ::
# Question
How to use CDPL in CF?
# Answer
# Note
## af_parse_config.cpp
```cpp
setenv("CDPL_LOGDIR", afdata_obj.getInternalRunDir().c_str(), 1)
setenv("CDPL_BCASTDIR", bcast_dir.c_str(), 1)
```
## af_masterflow.cpp
```cpp
using cdpl::Master;
using cdpl::Job;
using cdpl::Task;
std::vector<TaskInfo>& ts; // task want to run, user data include sh file name
static Master ms;
CdplExitCode err_code;
// collect ENV's into env_cvec
err_code = ms.disableWorkerLog();
err_code = ms.setExport(&env_cvec[0]); // export enviroment
ms.setHeartbeatRate(heartbeat_rate);
ms.setWorkerHeartbeatTimeout(heartbeat_timeout);
err_code = ms.init();
// check host file
// 1|localhost| 8| /tmp| SH| sh
err_code = ms.addHost("localhost", "/tmp", CDPL_SH, 8, "sh");
ms.setSubmitTimeout(submission_timeout);
ms.setSubmitTimeout(submission_timeout, CDPL__SGE);
ms.setSubmitTimeout(submission_timeout, CDPL__PBS);
ms.setMaxAttempts(task_max_retry_attempts);
err_code = ms.start();
Job job(&ms, &err_code);
err_code = job.addWorker(WORKER_TYPE,         /* arbitry */
			   1,		        /* min number of workers */
			   lic_mgr.num_slot(),	/* max number of workers */
			   worker_script.c_str()); /*START_WORKER customfault -worker*/
err_code = job.start();
Task *tsk = NULL;
while (nextTaskNumber < ts.size() || !job.isDone() || !task_cnt.is_done()) {
  while (slot_availabe() && nextTaskNumber < ts.size()) {
    tsk = new Task(&job, ts.at(nextTaskNumber).cshfile, "textfield", WORKER_TYPE, 0, &err_code);
    taskv.push_back(tsk);
    newTasksCreated = true;
    alloc_slot(tsk->getID());
    task_cnt.add_created(tsk->getID());
    nextTaskNumber++;
  }
  if (newTasksCreated) {
	  err_code = job.scheduleTasks();
  }
   CdplJobStatus js = job.getStatus();
   if (js) {
	   //...
   }
   for (it = taskv.begin(); it != taskv.end(); ) {
        bool tskDeleted = false;
		tsk = *it;
		// get the task status
	    CdplTaskStatus status = tsk->getStatus(NULL);
	    if (status == CDPL_TASK_COMPLETED) {
		    char const * textfield = NULL;
            CdplExitCode cdplerr = tsk->getText(&textfield);
	    }
	    else if(...) {
	    } 
	    it++
   }
   for (unsigned int i = 0; i < taskv.size(); ++i) {
      tsk = taskv.at(i);
      tsk->kill();
    }
    while(!job.isDone(&err_code)) {
      sleep(3);
    }
    err_code = job.finalize();
    err_code = ms.shutdown();
}
```
## af_workflow.cpp
```cpp
WorkerFlow::WorkerFlow() : worker(NULL), m_timeout(120000)
{
worker = new Worker();
CdplExitCode err_code = worker->init();
 err_code = worker->setSignal(1);
}

void WorkerFlow::execute()
{
 CdplExitCode err_code = worker->start();
  while (worker->isAlive()) {
	  while ((err_code = worker->waitForTasks(1000)) == CDPL__ETIMEDOUT) {
	  }  
  }
  err_code = worker->retrieveTask();
  process_task();
  err_code = worker->finalizeTask(0);
  err_code = worker->shutdown();
}
void WorkerFlow::process_task()
{
 CdplExitCode err_code = worker->getTaskCommand(&inpfile);
 std::string job_output = runSimulationWithSecureToken(inpfile);
 worker->updateTaskText(linebuf);
}
```
## Xa cdpl flow
```
cdplInitMaster
cdplStartMaster
cdplCreateJob
cdplAddWorker
cdplCreateTask
cdplShutdownMaster
cdplGetTaskRetval
cdplGetTaskInfo
cdplScheduleTasks
cdplStartJob
cdplFinalizeJob
cdplInitWorker
cdplGetData
cdplStartWorker
cdplSafeShutdownWorker
cdplWaitForWork
cdplGetText
cdplShutdownWorker
```
## check cdpl information
``` bash
Need to check worker log to find the problem
```