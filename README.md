# Low-Frequency Electrical Stimulation and interseizure delay: a biophysical and mathematical representation (PYTHON/XPPAUT)

Authors : G. Girier, I. Dallmer-Zerbe, J. Chvojka, J. Kudlacek, P. Jiruska, J. Hlinka H. Schmidt.

Contact : guillaumegirier@gmail.com

## Abstract :

The biological mechanisms underlying the spontaneous and recurrent transition to seizures in the epileptic brain are still poorly understood. As a result, seizures remain uncontrolled in a substantial proportion of patients with drug-refractory epilepsy. Brain stimulation is an emerging and promising method to treat various brain disorders, including drug-refractory epilepsy. Selected stimulation protocols demonstrated therapeutic efficacy in reducing the seizure rate successfully. The stimulation efficacy critically depends on chosen stimulation parameters, such as the time point, amplitude, and frequency of stimulation. This study aims to explore the neurobiological impact of 1 Hz stimulation and provide the mechanistic explanation behind its seizure-delaying and seizure-suppressing effects. We study this effect using a computational model, a modified version of the Epileptor-2 model, in close comparison with spontaneous seizures recorded \textit{in vitro} in a high-potassium model of ictogenesis in rat hippocampal slices.  In particular, we investigate 1) the mechanisms and dynamics of spontaneous seizure emergence, 2) the seizure-delaying effect of the stimulation, and 3) the optimal stimulation parameters to achieve the maximal anti-seizure effect. We show that the modified Epileptor-2 model can replicates key experimental observations and captures seizure dynamics and the anti-seizure effects of low-frequency electrical stimulation (LFES) observed in hippocampal slices. We identify the critical thresholds in the model for seizure onset and determine the optimal stimulation parameters—timing, amplitude, and duration—that exceed specific thresholds to delay seizures without triggering premature seizures. Our study highlights the central role of sodium-potassium pump dynamics in terminating seizures and mediating the LFES effect. The obtained results pave the road to a mechanistic framework to optimize stimulation protocols and deepen our understanding of brain stimulation to design new and effective brain stimulation strategies to treat epilepsy.

## Model :

We chose a modified version of the Epileptor-2 model [1]:

$$
  \tau_m \cdot \frac{dV}{dt} = u(V,x_{\textup D},Ko)-g_{\textup L} V + g_{\textup{stim}} I_{\textup{stim}}(t),\\
$$

$$
  \frac{dxD}{dt} = \frac{(1-x_{\textup D})}{\tau_x} - \delta_x   x_{\textup D}   \upsilon_i(V), \\
$$

$$
  \frac{d Ko}{dt} = \frac{K_{\textup{bath}}-Ko}{\tau_{K}} -2   \gamma   I_{\textup{pump}}(Ko,Nai)+ \delta_{K}   \upsilon_i(V),\\
$$

$$
  \frac{dNai}{dt} = \frac{Nai^0-Nai}{\tau_{Na}} -3   I_{\textup{pump}}(Ko,Nai)+\delta_{Na}   \upsilon_i(V), \\
$$

where $V$ is the average membrane potential of pyramidal cells, $x_{\textup D}$ is the synaptic resource, and $Ko$ and $Nai$ are the extracellular potassium and intracellular sodium concentrations, respectively. 

The membrane potential $V$ is driven by the potassium concentration $Ko$ and synaptic input, which is concisely expressed by

$$
    u(V,x_{\textup D},Ko) = g_K  26.6 \text{mV}  \log \left ( \frac{Ko}{Ko^0} \right )+I_{exc}(V,x_{\textup D}) + I_{inh}(V),
$$

where the first term is the potassium depolarizing current, and $I_{exc}$ and $I_{inh}$ are the excitatory and inhibitory synaptic inputs to the excitatory population. The excitatory synaptic input $I_{exc}$ is defined as:

$$
    I_{exc} (V,x_{\textup D}) = g_{exc} \upsilon_i(V) x_{\textup D},
$$

with $g_{exc}$ being the conductance of excitatory synapses, and $\upsilon_i(V)$ being the firing rate of the excitatory population.
The inhibitory synaptic input $I_{inh}$ is defined as [2]:

$$
    I_{inh} (V) = g_{inh}   (V_{inh} - V),
$$

with $g_{inh}$ being the conductance of inhibitory synapses, and $V_{inh}$ their reversal potential.

The firing rate function $\upsilon_i(V)$ is chosen to be a sigmoid function with threshold $V_{th}$ and maximum firing rate $\upsilon_{max}$:

$$
    \upsilon_i(V) &= \frac{\upsilon_{max}}{1+\e^{V_{th}-V}}.\\
$$

The excitatory population is also subjected to low-frequency stimulation, $I_{stim}(t)$. 
Each stimulus is represented by a Dirac delta function, and the sequence of stimuli thus reads $I_{stim}(t) = A \sum_n \delta(t-t_n)$, where $t_n$ represents stimulation times (every 1s in our protocol), and $A$ is the stimulation amplitude. 
The parameter $g_{stim}$ was added to ensure dimensional consistency within the model, thereby preserving the physical coherence of the equations and their underlying biophysical interpretation.

Lastly, the activity of the Na-K pump is computed as follows:

$$
    I_{pump}(Ko,Nai) &=  \frac{\rho}{(1+\e^{3.5-Ko}) (1+\e^{(25.0-Nai)/3})}. \\
$$

## Repository organization :

In this github, several notebooks are proposed:

1) In **1 - Modified Epileptor-2 : applying the Low-Frequency Electrical Stimulation (LFES) protocol**, we present the Epileptor-2 model and illustrate the various time series it generates under different conditions. We first demonstrate the model’s behavior in the absence of low-frequency electrical stimulation (LFES), providing insights into the baseline seizure dynamics. Subsequently, we introduce LFES into the model and showcase its effects on the time series, highlighting the modulation of seizure activity and the underlying mechanisms at play.

2) In **2 - Adding potassium in the medium with a slow ramp**, we investigate the effect of extracellular potassium concentration (Kbath) on the dynamics of the computational model, emphasizing its role in shaping seizure-like activity. Using a simulated protocol of slow potassium addition, we analyze how gradual changes in Kbath influence the model’s behavior. The notebook highlights key features of the model’s response, including the generation of time-series data under varying Kbath conditions and the impact of single-spike stimulations at A=30 mA. Detailed visualizations are provided to illustrate the transition between different dynamical states, offering a mechanistic perspective on how potassium dynamics modulate neuronal excitability and seizure onset. (Corresponding to Fig.2)

3) In **3 - Bifurcation diagram plots**, we generate a 3D bifurcation diagram in the {[K+]o,[Na+]i,V}$phase space, derived from the fast subsystem equations. This bifurcation diagram provides a detailed view of the system's dynamical structure, highlighting the transitions between different states as governed by the interplay of extracellular potassium [K+]o, intracellular sodium [Na+]i, and membrane potential V. Using the bifurcation tool XPPAUT, we compute the bifurcation surfaces (the data is available in the file "dia_bif.dat", and the code to produce it is named "Epileptor2_fastsub.ode"), which are overlaid with a non-stimulated solution trajectory obtained from the full system equations.

4) In **4 - Breaking down the time series in a {Na, K} phase plan**, we analyze the effects of stimulation on the system dynamics by representing the trajectories of the model in the {[Na+],[K+]} phase plane. Using the full system equations, we explore the response of the model to stimulation protocols with a chosen amplitude (the user can change the parameter A). Multiple stimulated time series are visualized as arrowed curves, capturing the system's evolution under repeated stimulations, while colormaps provide a detailed representation of a single time series. For clarity, the onset of each stimulation is marked by a dot, and the stimulation duration is highlighted in red.


## Prerequisites :

Python : v. 3.12.4

Numpy : v. 1.26.0

Matplolib : v. 3.9.2

Scipy : v. 1.13.1

## Bibliography :

[1] Chizhov AV, Zefirov AV, Amakhin DV, Smirnova EY, Zaitsev AV (2018) Minimal model of interictal and ictal discharges “Epileptor-2”. PLOS Computational Biology 14(5): e1006186. https://doi.org/10.1371/journal.pcbi.1006186

[2] Börgers, C., Krupa, M. & Gielen, S. The response of a classical Hodgkin–Huxley neuron to an inhibitory input pulse. J Comput Neurosci 28, 509–526 (2010). https://doi.org/10.1007/s10827-010-0233-8


## License :

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not, see <https://www.gnu.org/licenses/>. 3



