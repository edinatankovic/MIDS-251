
# Homework 09 - Distributed Training and Neural Machine translation

This homework covers training a transformer based machine translation network on a small English to German WMT corpus. The training is completed on two VMs on IBM cloud. The command line that created both machines are below:

## IBM cloud commands:

ibmcloud sl vs create --datacenter= wdc07 --hostname=v100a --domain=edina.com --image=2263543 --billing=hourly  --network 1000 --key=1595886 --flavor AC2_16X120X100 –san


ibmcloud sl vs create --datacenter= wdc07 --hostname=v100b --domain=edina.com --image=2263543 --billing=hourly  --network 1000 --key=1595886 --flavor AC2_16X120X100 –san


### Submission: Assignment questions answered:

Please submit the nohup.out file along with screenshots of your Tensorboard indicating training progress (Blue score, eval loss) over time.  

The **nohup.out** file is uploaded to this repository.
## The Graphs:

NOTE: "Ignore outliers in chart scaling" was selected.

 **BLEU Graph**
 
![BLEU - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/1_1_Eval_BLEU_Score.png)

 **Gradient Loss**
 
![Gradient Loss - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/2_GlobalGradientLossScale.png)

 **Eval Loss**
 
![Eval Loss - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/3_eval_loss.png)


 **Global Step**
 
![Global Step - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/4_globalStep.png)

 **Learning Rate**
 
![Learning Rate - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/5_Learning%20rate.png)


 **Train Loss**
 
![Train Loss - 1 VM](https://github.com/edinatankovic/MIDS-251/blob/master/HW9/Images/6_TrainLoss.png)

Also, answer the following (simple) questions:
* How long does it take to complete the training run? (hint: this session is on distributed training, so it *will* take a while)
  **Answer: the training took 21h 38m 48s, it was limited to 50,000 steps, I made change in the cofig file**
* Do you think your model is fully trained? How can you tell?
 **Answer: the last point on the BLEU graph is still moving, I suspect the training is not finished**
* Were you overfitting?
  **Answer: Possibly, the eval loss has been approaching a constant value**
* Were your GPUs fully utilized?
  **Answer: Possibly**
* Did you monitor network traffic (hint:  ```apt install nmon ```) ? Was network the bottleneck?
  **Answer: Possibly**
* Take a look at the plot of the learning rate and then check the config file.  Can you explan this setting?
  **Answer: there is a warmup setting set to 8k (lr_policy_params: warmup_steps in /data/transformer-base.py, which is why it increases at first and then it decays normally**
* How big was your training set (mb)? How many training lines did it contain?
  **Answer - file size**:
  
  root@v100a:/data/wmt16_de_en/data# du -h /data/wmt16_de_en/
  
  64M	/data/wmt16_de_en/data/dev/dev
  
  64M	/data/wmt16_de_en/data/dev
  
  11M	/data/wmt16_de_en/data/test/test
  
  11M	/data/wmt16_de_en/data/test
  
  2.5G	/data/wmt16_de_en/data/common-crawl
  
  211M	/data/wmt16_de_en/data/nc-v11/training-parallel-nc-v11
  
  211M	/data/wmt16_de_en/data/nc-v11
  
  588M	/data/wmt16_de_en/data/europarl-v7-de-en
  
  4.5G	/data/wmt16_de_en/data
  
  14G	/data/wmt16_de_en/
  
  **Answer - line count**:
  root@v100a:/data/wmt16_de_en# wc -l  train.de
  
  4562102 train.de
  
  root@v100a:/data/wmt16_de_en# wc -l  train.en
  
  4562102 train.en


* What are the files that a TF checkpoint is comprised of?
  **Answer** Basically they contain a path to the file that has the last model saved down. The .ckpt is a binary file which contains all the values of the weights, biases, gradients and all the other variables saved. There is also a file named checkpoint which simply keeps a record of latest checkpoint files saved.
  Here is a sample output:

  model_checkpoint_path: "model.ckpt-0"

  all_model_checkpoint_paths: "model.ckpt-0"

* How big is your resulting model checkpoint (mb)?
  **Answer**: 852 Mb.
  
  root@v100a:/data/en-de-transformer# ls -lrt *ckpt*
  
   -rw-r--r-- 1 root root     36131 Oct 20 17:24 model.ckpt-0.index
   
   -rw-r--r-- 1 root root 852267044 Oct 20 17:24 model.ckpt-0.data-00000-of-00001
   
   -rw-r--r-- 1 root root  15374541 Oct 20 17:24 model.ckpt-0.meta
   
* Remember the definition of a "step". How long did an average step take?
   
   **Answer**:  Step is one iteration of gradient descent learning algorithm. If I divide the time it took to run, with the number of steps (50,000) I get 1.558 seconds.
    
* How does that correlate with the observed network utilization between nodes?
