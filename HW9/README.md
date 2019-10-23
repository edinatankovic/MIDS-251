
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
* Were you overfitting?
* Were your GPUs fully utilized?
* Did you monitor network traffic (hint:  ```apt install nmon ```) ? Was network the bottleneck?
* Take a look at the plot of the learning rate and then check the config file.  Can you explan this setting?
* How big was your training set (mb)? How many training lines did it contain?
* What are the files that a TF checkpoint is comprised of?
* How big is your resulting model checkpoint (mb)?
* Remember the definition of a "step". How long did an average step take?
* How does that correlate with the observed network utilization between nodes?
