# rllib-memory-leak

## Reproduction
OS: Ubuntu 18.3
1. `python generate_data.py`
2. `rllib train -f marwil.yaml` or `rllib train -f marwil-gpu.yaml`

## Error log
```
2020-09-19 18:35:17,052	ERROR trial_runner.py:567 -- Trial MARWIL_PongNoFrameskip-v4_3aaf9_00000: Error processing event.
Traceback (most recent call last):
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/tune/trial_runner.py", line 515, in _process_trial
    result = self.trial_executor.fetch_result(trial)
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/tune/ray_trial_executor.py", line 488, in fetch_result
    result = ray.get(trial_future[0], timeout=DEFAULT_GET_TIMEOUT)
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/worker.py", line 1429, in get
    raise value.as_instanceof_cause()
ray.exceptions.RayTaskError(RayOutOfMemoryError): ray::MARWIL.train() (pid=11474, ip=192.168.0.15)
  File "python/ray/_raylet.pyx", line 446, in ray._raylet.execute_task
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/memory_monitor.py", line 140, in raise_if_low_memory
    self.error_threshold))
ray.memory_monitor.RayOutOfMemoryError: More than 95% of the memory on node suwi is used (30.47 / 31.3 GB). The top 10 memory consumers are:

PID	MEM	COMMAND
11474	26.28GiB	ray::MARWIL
10244	0.91GiB	/snap/pycharm-professional/215/jbr/bin/java -classpath /snap/pycharm-professional/215/lib/bootstrap.
2353	0.39GiB	/usr/bin/gnome-shell
11384	0.25GiB	/home/julius/anaconda3/envs/ray/bin/python /home/julius/anaconda3/envs/ray/bin/rllib train -f marwil
1452	0.13GiB	/usr/bin/gnome-shell
3488	0.13GiB	/opt/google/chrome/chrome
3712	0.1GiB	/opt/google/chrome/chrome --type=renderer --field-trial-handle=6849457355709256574,34731164304076937
9109	0.09GiB	/opt/google/chrome/chrome --type=renderer --field-trial-handle=6849457355709256574,34731164304076937
3526	0.09GiB	/opt/google/chrome/chrome --type=gpu-process --field-trial-handle=6849457355709256574,34731164304076
2810	0.09GiB	/usr/bin/gnome-software --gapplication-service

In addition, up to 0.2 GiB of shared memory is currently being used by the Ray object store. You can set the object store size with the `object_store_memory` parameter when starting Ray.
---
--- Tip: Use the `ray memory` command to list active objects in the cluster.
---
== Status ==
Memory usage on this node: 31.0/31.3 GiB: ***LOW MEMORY*** less than 10% of the memory on this node is available for use. This can cause unexpected crashes. Consider reducing the memory used by your application or reducing the Ray object store size by setting `object_store_memory` when calling `ray.init`.
Using FIFO scheduling algorithm.
Resources requested: 0/6 CPUs, 0/1 GPUs, 0.0/16.6 GiB heap, 0.0/5.71 GiB objects (0/1.0 accelerator_type:RTX)
Result logdir: /home/julius/Github/rllib-memory-leak/results/marwil
Number of trials: 1/1 (1 ERROR)
+---------------------------------------+----------+-------+--------+------------------+--------+----------+----------------------+----------------------+--------------------+
| Trial name                            | status   | loc   |   iter |   total time (s) |     ts |   reward |   episode_reward_max |   episode_reward_min |   episode_len_mean |
|---------------------------------------+----------+-------+--------+------------------+--------+----------+----------------------+----------------------+--------------------|
| MARWIL_PongNoFrameskip-v4_3aaf9_00000 | ERROR    |       |    498 |          64.8622 | 458820 |      nan |                  nan |                  nan |                nan |
+---------------------------------------+----------+-------+--------+------------------+--------+----------+----------------------+----------------------+--------------------+
Number of errored trials: 1
+---------------------------------------+--------------+----------------------------------------------------------------------------------------------------------------------------+
| Trial name                            |   # failures | error file                                                                                                                 |
|---------------------------------------+--------------+----------------------------------------------------------------------------------------------------------------------------|
| MARWIL_PongNoFrameskip-v4_3aaf9_00000 |            1 | /home/julius/Github/rllib-memory-leak/results/marwil/MARWIL_PongNoFrameskip-v4_3aaf9_00000_0_2020-09-19_18-34-07/error.txt |
+---------------------------------------+--------------+----------------------------------------------------------------------------------------------------------------------------+

Traceback (most recent call last):
  File "/home/julius/anaconda3/envs/ray/bin/rllib", line 8, in <module>
    sys.exit(cli())
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/rllib/scripts.py", line 34, in cli
    train.run(options, train_parser)
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/rllib/train.py", line 214, in run
    concurrent=True)
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/tune/tune.py", line 486, in run_experiments
    scheduler=scheduler).trials
  File "/home/julius/anaconda3/envs/ray/lib/python3.7/site-packages/ray/tune/tune.py", line 434, in run
    raise TuneError("Trials did not complete", incomplete_trials)
ray.tune.error.TuneError: ('Trials did not complete', [MARWIL_PongNoFrameskip-v4_3aaf9_00000])

```