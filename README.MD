# Qubit characterization and calibration on a RFSoC-based control system

## The long journey from the hardware setup to a possible quantum sensing application, developing an economic alternative to commercial control devices

### SUMMARY:

Quantum computing has attracted significant interest from its early beginnings, even when there were no physical implementations of quantum computers yet.
The proposal and development of algorithms (such as those by Shor and Grover), that are theoretically more efficient than what can be designed with classical computers, have led to significant progress in this field over the last decade.


These early discoveries have led to an intensification of research and development, aided by the active involvement of private companies, such as Google and IBM, which, within a few years, have invested in new technologies and built computers efficient enough to claim the achievement of a "Quantum Primacy".
%
Although these advancements have yielded noticeable results, current computers are still not precise enough to be practically useful.
Indeed, we are in what is referred to as the "Noisy Intermediate-Scale Quantum" (NISQ) era, where quantum computers exist, but are too noisy to support quantum algorithms without encountering errors.
%
For this reason, after years of research focusing on increasing the number of qubits (the quantum computing equivalent of classical bits), current research is shifting its focus toward designing and building more reliable and efficient individual qubits.
%
In order to achieve this goal, research is working simultaneously in multiple directions: experimenting with various types of qubits (superconducting, trapped ions, etc.), trying to enhance the highly delicate qubit fabrication processes, and improving the methods for qubit control and operation.
%
My thesis work is part of the research concerning the last point: Quantum Control, specifically for superconducting qubits.

A superconducting qubit can be represented as a LC circuit where the classical inductance is replaced by a non-linear inductance obtained with a Josephson junction.
This junction exploit the phenomenon of superconductivity to create a non-harmonic Hamiltonian, allowing the separation of the first two states (those used for computation) from the others.
The qubit is then coupled to a classical resonator, described by a harmonic Hamiltonian, which is used for readout.
%
All transition frequencies of this system, which can be precisely studied within the framework of cQED (circuit Quantum ElectroDynamics), are typically on the order of GHz.
%
By applying voltage pulses at these frequencies, it is possible to read the state of the qubit and modify it as desired.
%
However, synthesizing the required pulses is itself a technological challenge, as typical Digital to Analog Converters (DACs) reach at most hundreds of MHz, requiring up/down-conversion schemes that necessitate the use of additional equipment.
This complicates the experimental setup and introduce additional sources of noise into a system that is highly sensitive to external disturbances.
%
All parameters describing the pulses are precisely calibrated to maximize the qubit's performance (relaxation time $T_1$, dephasing time $T_2$, accuracy), and even minor variations can significantly affect coherence times or lead to leakage outside the computational space.
This requires the use of specialized instruments.

Several companies have developed proprietary tools for the synthesis and definition of these pulses.
However, these tools are often expensive, and researchers are frequently constrained in signal quality due to typical up/down-conversion schemes.
Additionally, each instrument is controlled using different software and programming languages, which complicates integration into a unified control and readout framework.

A recent, more versatile, and cost-effective alternative is the use of RFSoC boards (Radio Frequency System on Chip).
These boards are a specific type of FPGA (Field Programmable Gate Arrays) composed of programmable integrated circuits, all connected on a single board, which also incorporates several high-performance RF components.
The most notable feature of RFSoC boards is their ability to synthesize GHz pulses with great precision and full control, achieved through a technique known as Direct Digital Synthesis (DDS).
%
RFSoC boards, initially developed for communications purposes, have quickly become appealing also for quantum computing due to their synthesis capabilities and the inclusion of a single electronic board that houses a CPU, multiple high-performance DACs, and ADCs.
Additionally, since 2020, the Fermilab's Qick project has provided open-source firmware, enabling the programming of these FPGAs for qubit control and readout purposes.

In my thesis work, I utilized Qick as a foundation to develop Qibosoq, an open-source software designed for the control and readout of superconducting qubits using RFSoC boards.
Additionally, Qibosoq integrates RFSoC boards into the Qibo framework, a high-level software platform that allows for the definition of quantum algorithms and circuits to be executed on hardware later on.

During my thesis period at the Technology Innovation Institute (TII) in Abu Dhabi, I focused on the characterization and calibration of qubits, by using Qibosoq in conjunction with various RFSoC boards for the control of single fixed-frequency qubits (physically implemented with a single Josephson junction in a 3D cavity) and for the simultaneous control of multiple variable-frequency qubits (constructed with pairs of Josephson junctions coupled to planar resonators).

%
The characterization of qubits is a process that involves various experiments aimed at identifying all the key parameters specific to the qubit itself and to the resonant cavity to which it is coupled for readout.
These parameters are then used along with those obtained from additional calibration experiments to optimize the control of the qubit, making it suitable for applications in quantum computing or quantum sensing.
The sequence of the experiments required is typically composed of at least fifteen different experiments.
Each of these experiments involves the synthesis of specific pulse sequences and measurements aimed at optimizing one or more of the parameters that describe the system.
%
For example, the Ramsey experiment, which allows the identification of the value of T_2 (the decay constant describing the loss of coherence of the qubit system), is performed by synthesizing two consecutive pulses that take the qubit from a defined state (0-1) to a superposition and vice versa, using an operation described by \frac{1}{\sqrt{2}}\begin{pmatrix}1 & 1 \\ i & i \end{pmatrix}\).
The two pulses are separated by a variable delay, and a relative phase of \pi/2 is added to the second pulse.
%
This process enables the examine, as a function of the delay between the two pulses, the typical dephasing decay that occurs following the first pulse.
The pulses must be synthesized at the characteristic frequency of the qubit, and even small deviations can manifest as oscillatory precessions in the qubit state.
For this reason, this procedure can also be used to precisely identify the qubit's transition frequency.

%Questo esempio illustra bene come un singolo esperimento possa risultare complesso da realizzare dal punto di vista sperimentale.
%
This example effectively illustrates how a single experiment can be quite complex to implement from an experimental perspective.
In this regard, a system based on RFSoC boards, such as the one developed for this thesis, which does not require additional instruments beyond the board itself, can have a significant impact in simplifying the characterization and calibration operations.
Furthermore, it's important to note that once a qubit control system is developed, it can be used for a wide range of experiments beyond just calibration and characterization.

%
The work at TII has always been focused on quantum computing, meaning the use of qubits for the execution of quantum algorithms.
In this context, the developed system has already been used to run quantum machine learning algorithms aimed at estimating Parton Distribution Functions (PDFs) on qubits.
%
However, in the future, it will also become an important tool for applications in quantum sensing, using qubits as particle detectors.

%
In particular, in 2021 the italian INFN (Istituto Nazione di Fisica Nucleare) presented the Qub-IT project, which aims to use qubits for the search for dark matter, specifically for dark photons and axions.
The fundamental idea behind Qub-IT is to couple a qubit to a cavity that, in resonance with potential dark matter particles, could induce the conversion of these particles into photons.
The various photons present in the cavity influence the coupled qubit, making it possible to measure the number of photons with high precision through a specific pulse sequence not too dissimilar from the one required in the previously mentioned Ramsey experiment.
%
In the project proposal document, one of the key points is the development of a qubit control and readout system using FPGAs that is precise, reliable, and scalable.
%
The work of my thesis represents an important step in this direction.

%
The thesis provides the theoretical elements of cQED necessary to understand how the control of superconducting qubits works.
It illustrates the experimental setup and hardware configuration required for the various RFSoC boards used, along with a detailed explanation of the unique features of RFSoC boards, including the synthesis and acquisition methods used to work directly in the GHz range.
Additionally, the software Qibosoq and Qibolab, developed and used during the thesis work, are briefly introduced and commented upon.
%
The core of the thesis consists of a detailed explanation of the main experiments required for the characterization and calibration of single qubits and two-qubit gates.
Each experiment includes graphs to be obtained under ideal conditions, some real-world graphs, and solutions to some of the most common experimental problems.
This section is designed to potentially serve as a practical manual for the calibration and characterization of superconducting qubits.
%
Finally, the thesis concludes with two examples of possible applications of the developed control and readout system: the first in quantum machine learning (already tested on hardware), and the second in quantum sensing, related to the experiment for the search for dark matter.
