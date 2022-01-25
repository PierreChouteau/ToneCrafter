<h1 align=left><img src="ToneCrafter_logo.png" width="60">&emsp;ToneCrafter</h1>

ToneCrafter is a project developped by students at [ENSEA](https://www.ensea.fr/).  
Our goal is to develop an algorithm capable of analyzing any audio file containing guitar sounds and retrieve its parameters. ToneCrafter will then generate a numeric filter that will modulate your guitar sound and make it sound like the recording you fed it. Exciting isn't it ?  
We are also working on a hardware implementation using a STM32 µC that will allow musicians to use ToneCrafter on stage and tune the different parameters.

- [Our work so far](#our-work-so-far)
  * [Software](#software)
  * [Hardware](#hardware)
- [How to use ToneCrafter](#how-to-use-tonecrafter)
  * [Installing Anaconda and TensorFlow](#installing-anaconda-and-tensorflow)
  * [Dataset](#dataset)
- [What's next ?](#whats-next-)
- [What's new ?](#whats-new--25012022)

## Our work so far
### Software
We started out trying to use Magenta's [DDSP](https://www.github.com/magenta/ddsp), but this was a bit too ambitious for AI newbies like us, so we tried implementing our own algorithms.  
This sparked three different approaches to our problem:  
  * A CNN based approach where we tried to teach a neural network to recreate a distortion.
  * A "bare coding" approach using math and clever algorithms to recreate a distortion from an audio file
  * A VAE based approach trying to recreate the mechanisms behind Magenta's DDSP

### Hardware
We started working on different PCB designs using [Eagle](https://www.autodesk.com/products/eagle/overview) and working on a STM32F7 discovery kit for testing purposes.

## How to use ToneCrafter
### Installing Anaconda and TensorFlow
To begin with, if you want to work locally and not on Colabs as we did, you will have to install Anaconda.
To do this follow the [officials instructions](https://docs.anaconda.com/anaconda/user-guide/tasks/tensorflow/) that are given to you.  

If you want to try the huge potential of DDSP, let’s check the [DDSP GitHub](https://github.com/magenta/ddsp) which show how to install it.
There are also some tutorials to introduce the main concepts of the library (that we strongly recommend you to do) and demos to show what you can do with it (Like Timbre transfer, Train Autoencoder and Pitch Detection).


And then, if you want to have fun on the models created by the Google team with the [Magenta](https://magenta.tensorflow.org/) project, follow the installation guide on their [page](https://github.com/magenta/magenta).


### Dataset
To train our CNN, we searched for a dataset containing clean audio from a guitar and different distortions. As we could not find any we had to create our own. We recorded a guitar for 1 minute and added various levels of distortion. To increase the size of our base, we applied an EQ filter with various settings.  
We ended up with a lot of .wav files that you can find [here](https://github.com/ToneCrafter-Team/ToneCrafter/tree/main/Software/CNN%20Models/Dataset).  
In order to train our CNN, we splitted our files in 200ms chunks using our [Preparing Data Notebook](https://github.com/ToneCrafter-Team/ToneCrafter/blob/main/Software/CNN%20Models/Preparing_Data.ipynb)  
We then organised our files in the following way :  

- X_train :  
  * Clean  
  * Clean_TrebbleBoost  
  * Clean_BassBoost  
  * Clean_BassCut  

- y_train :
  * Disto  
  * Disto_TrebbleBoost  
  * Disto_BassBoost  
  * Disto_BassCut  

- Validation data :
  * Clean_TrebbleCut as X_valid  
  * Disto_TrebbleCut as y_valid  

We are concious that this dataset is far from perfect (please don't listen to it, the guitar playing is awful) from a recording standpoint and the way we splitted our audio might cause some problems.

For the maths oriented approach, we used the excellent [IDMT-SMT-Audio_Effects Dataset](https://www.idmt.fraunhofer.de/en/business_units/m2d/smt/audio_effects.html), which enabled us to link each note played to it's distorted counterpart.  

<span style="color:red"> *For a more in-depths version of this readme, checkout our (magnificent) project report available in both French and English at the root of this repository*</span>
## What's next ?
As of the 03/05/2021, our yearly project has ended. We will continue to work on ToneCrafter at a lighter pace, as we'll be completing our summer internships. If all goes well, some new students will pick up the project in the fall to continue to build on top of our work. In the mean time, don't hesitate to take part in our project, via issues or pull requests.  

## What's new ? (25/01/2022)
For this new year, I (Pierre Chouteau) continued the project for my final year at the ENSEA. 
Concretely, I have mainly advanced the Software part. To create an accurate model, I based myself on the article and the code of Mr Keith Bloemer and the paper of Aalto, which both use an artificial intelligence model based on LSTMs. This model allows a good reproduction of a distortion effect.    
The origin of the model, its creation, the dataset used as well as the training and the results are detailed in the [report](https://github.com/PierreChouteau/ToneCrafter/blob/main/Report/2022/%5BFR%5D_Final_Report_2022.pdf).

The part I really focused on this year is the implementation of this model, not on Raspberry like Mr Bloemer did, but directly on STM32, as close as possible to the hardware. This will allow to gain in performance, when the code will be really functional. 

In order to understand the implementation of a neural network on STM, in collaboration with Elisa DELHOMME, we started by implementing a simpler model of sine prediction. It turns out, after different tests, that it works correctly, both under Keras and on the STM. You can see this work [here](https://github.com/PierreChouteau/ToneCrafter/tree/main/Software/Implementation%20on%20STM32/Sinus%20Prediction%20on%20STM).   
In a second step, I tried to implement the LSTM network found earlier, but there are still some problems to solve... See this part [here](https://github.com/PierreChouteau/ToneCrafter/tree/main/Software/Implementation%20on%20STM32/ToneCrafter%20on%20STM).
