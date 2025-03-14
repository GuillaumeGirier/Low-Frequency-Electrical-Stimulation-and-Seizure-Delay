# Low-Frequency Electrical Stimulation and interseizure delay: a biophysical and mathematical representation (PYTHON/XPPAUT)

Authors : G. Girier, I. Dallmer-Zerbe, J. Chvojka, J. Kudlacek, P. Jiruska, J. Hlinka H. Schmidt.

Contact : guillaumegirier@gmail.com

## Abstract :

The biological mechanisms underlying the spontaneous and recurrent transition to seizures in the epileptic brain are still poorly understood. As a result, seizures remain uncontrolled in a substantial proportion of patients with drug-refractory epilepsy. Brain stimulation is an emerging and promising method to treat various brain disorders, including drug-refractory epilepsy. Selected stimulation protocols demonstrated therapeutic efficacy in reducing the seizure rate successfully. The stimulation efficacy critically depends on chosen stimulation parameters, such as the time point, amplitude, and frequency of stimulation. This study aims to explore the neurobiological impact of 1 Hz stimulation and provide the mechanistic explanation behind its seizure-delaying and seizure-suppressing effects. We study this effect using a computational model, a modified version of the Epileptor-2 model, in close comparison with spontaneous seizures recorded \textit{in vitro} in a high-potassium model of ictogenesis in rat hippocampal slices.  In particular, we investigate 1) the mechanisms and dynamics of spontaneous seizure emergence, 2) the seizure-delaying effect of the stimulation, and 3) the optimal stimulation parameters to achieve the maximal anti-seizure effect. We show that the modified Epileptor-2 model can replicates key experimental observations and captures seizure dynamics and the anti-seizure effects of low-frequency electrical stimulation (LFES) observed in hippocampal slices. We identify the critical thresholds in the model for seizure onset and determine the optimal stimulation parameters—timing, amplitude, and duration—that exceed specific thresholds to delay seizures without triggering premature seizures. Our study highlights the central role of sodium-potassium pump dynamics in terminating seizures and mediating the LFES effect. The obtained results pave the road to a mechanistic framework to optimize stimulation protocols and deepen our understanding of brain stimulation to design new and effective brain stimulation strategies to treat epilepsy.

## Model :

We chose a modified version of the Epileptor-2 model [1]:

$$
  \tau_m \cdot \frac{dV}{dt} = + g_{\textup{stim}} I_{\textup{stim}}(t),\\
$$

$$
  \frac{dxD}{dt} = \frac{(1-x_{\textup D})}{\tau_x} - \delta_x   x_{\textup D}   \upsilon_i(V), \\
$$

$$
  \frac{d [K^+]_{_{\textup o}}}{dt} = \frac{\K_{\textup{bath}}-[K^+]_{_{\textup o}}}{\tau_{K}} -2   \gamma   I_{\textup{pump}}([K^+]_{\textup o},[Na]_{\textup i})+ \delta_{K}   \upsilon_i(V),\\
$$

$$
  \frac{d[Na^+]_{_{\textup i}}}{dt} = \frac{[Na^+]_{\textup i}^0-[Na^+]_{\textup i}}{\tau_{Na}} -3   I_{\textup{pump}}([K^+]_{\textup o},[Na^+]_{\textup i})+\delta_{Na}   \upsilon_i(V), \\
$$

where Ko and Nai represent extracellular potassium and intraneuronal sodium concentrations, respectively; V is the membrane depolarization; xD is the synaptic resource; vi(t) is the firing rate of an excitatory population. We define the additional equations as follows :

$$
    vi(V) = vmax \cdot \frac{1}{1+e^{Vth-V}}\\
$$
$$
    u(V,xD,Ko) = gK\cdot 26.6\cdot \log(\frac{Ko}{Ko0})+G_{syn}\cdot vi(V)\cdot (xD-0.5)+ \sigma \cdot  \xi \\
$$
$$
    Ipump(Ko,Nai) =  \frac{\rho}{(1+e^{3.5-Ko})\cdot(1+e^{(25.0-Nai)/3})} \\
$$

where $\xi$ is random samples from a normal (Gaussian) distribution. (contain between 0 and 1)

### Adding the inhibitory current :

To show the delay between ictal phases, and therefore the phenomenon of depolarization during stimulation, then repolarization at the end of the spike train, we add an additional inhibitory term $I_{inh}$ defined as follows [2]:

$$
    I_{inh} = g_{inh} \cdot S_{inh} \cdot (V_{inh} - V)
$$

We suppose the existence of a passive population connected to the studied excitatory population, with the conductance, $g_{inh}$, and a gating parameter, $S_{inh}$.

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



