# Scripts used to Test and Benchmark FlowPM




## Benchmarking on TPUs

To run a benchmark on TPUs here are the steps to follow

0) Make sure your cloud VM environment is correctly setup, see the README TPU section

1) From a first VM shell, start the benchmarking script:
```
$ cd flowpm/scripts
$ python3 fft_benchmark_TPU.py --model_dir=gs://flowpm_eu/tpu_test
```

2) From a second VM shell, start Tensorboard as such:
```
$ export PATH="$PATH:`python3 -m site --user-base`/bin"
$ export STORAGE_BUCKET=gs://flowpm_eu/tpu_profiling/run0
$ export MODEL_DIR=gs://flowpm_eu/tpu_test
$ tensorboard --logdir=${MODEL_DIR} &
```
Finally, click the web preview button on the top right corner to launch TensorBoard

3) To capture the TPU trace, go to the profile tool, use these settings:
TPU name: flowpm
Address Type: TPU Name
Profiling duration: 10000

4) When you have acquired the profile, you can shutdown the running benchmarking
script with `ctrl+\`

Alternatively, one can also save the trace and then visualize it in tensorboard. To do so, 

1) Follow step 1) above and start the benchmarking script

2) From a second VM shell that has again been setup to start the TPU as pointed in main README, do the following:
```
$ export PATH="$PATH:`python3 -m site --user-base`/bin"
$ export TPU_NAME=flowpm
$ export MODEL_DIR=gs://flowpm_eu/tpu_test
$ capture_tpu_profile --tpu=$TPU_NAME --logdir=${MODEL_DIR} --duration_ms=10000 --num_tracing_attempts=10
```
Ideally this will end with some output like - _Profile session succeed for host(s):10.240.1.5,10.240.1.2,10.240.1.4,10.240.1.3_

3) From a third VM shell, follow the step 2) above to launch tensorboard and visualize


More info on using TensorBoard and Profiling for TPU here:
  - https://cloud.google.com/tpu/docs/tensorboard-setup
  - https://cloud.google.com/tpu/docs/cloud-tpu-tools

To access these traces from your local computer, here are the 2 simple steps
  - Use the gcloud cli to authenticate yourself:
  ```
  $ gcloud auth application-default login
  ```
  - Start TensorBoard with the path to your Bucket:
  ```
  $ tensorboard --logdir=gs://flowpm_eu/tpu_test
  ```
  - Step 3: Profit!
