*************************
On System Verilog Modules
*************************

*"module"* is the basic unit (block) of hierarchy in SystemVerilog and they can be reused to create other modules in hierarchical manner. The module describes the boundaries, inputs and outputs, and the functionality of the circuit block being modelled. To explain the concept of the module, a multiplexor circuit is modelled based on its gate level schematic. 

Multiplexer is a combinational logic circuit which outputs the data coming from one of its inputs as defined by the value of its selection signal. For example, a two input MUX has two data inputs (in0,in1) and a selector input (sel), and it has only one output (f ) (refer Figure 1 for the circuit).

.. figure:: ch1MUX.png
   :scale: 25 %
   :align: center

   Two input multiplexor symbol and its gate level schematic

.. code-block:: 
   :caption: CaptionHere

   module mux2gate(f, in0, in1, sel);
   // this is a comment
	   output logic  f;
	   input logic in0, in1, sel;
	   logic out1,nsel,out2; 

      /* This is a multi line 
      comment */

	   and u0 (out1,in1,sel); // <module type> <instance name> 
	   not u2 (nsel, sel); 
	   and u1 (out2,nsel,in0);
	   or u3 (f,out1,out2);
   endmodule: mux2gate

Like in C langauage, \SV commands and keywords must be written in lower-case letters, and all the identifiers are case-senstive - \texttt{a} and \texttt{A} are two different identifiers. 

Modules start with the keyword \texttt{module} followed by its name \textit{mux2gate} (line 1), given by the designer. 
\begin{lstlisting}[language=SystemVerilog,frame=none, numbers=none]
module mux2gate(f, in0, in1, sel);
\end{lstlisting}

Written within brackets (in1, in2, sel and f) are the interface port names through which module interacts with the outside (Note that in \SV there are modules without a port list, which do not generate any logic block, but they are used to test logic blocks desgined in \SV.) It is always a good idea to use meaningful names for both module name and port names. The line 1 in the module description is equivalent to the block level view of the component showing only its connections to outside, without highlighting the minute internal details. This line is similar to the MUX symbol in Figure \ref{fig:gatelevelMux} (a).  

In line 3 and 4, the nature of the port are described; f is an output port, while all other ports are inputs. Lines 1,3 and 4 can be combined and expressed as: \texttt{module mux2gate(output f, input in0, in1, sel)}. In \SV there are three types of I/O ports: \textit{input, output, inout} and \textit{ref}. Input ports are used to receive signals from outside or they can be connected to outputs from other modules. Output ports are to output values or to connect to inputs of other modules. The inout ports are to model bi-directional ports, to receive in or send out values from the module. ref is a module connection that don't model hardware, but it allows deginers to pass values by reference like in C language.

We are going to refer the gate level schematic of the circuit in Figure \ref{fig:gatelevelMux} and decribe the circuit using \SV. This step is similar to connecting discreet components according to a schematic. In order to do that we require some electrical wires, which aare defined in line 5. The keyword \texttt{wire} in line 5 specifies that the internal connections are \texttt{wire} type. \textit{out1,out2}, and \textit{nsel} are the outputs of AND gates U0 and U1, and NOT gate U2, respectively. 

Internal connections are explicitly declared in this case (in line 5) even though \SV allows implicit connections too. It is a good practise to declare internal connections because the simulators might give errors or produce unexpected results not being able to make the correct connection type (most of the cases the compliler will use a scalar connection where a multi-bit wire is required. Therefore, implicit connections are not always recommended.   

The logic gate connections are defined in lines 7-10. Basic logic gate primitives are available in \SV, where designers can use them when describing modules in gate level. For example, in line 7, a two input AND gate is used and its inputs are \textit{in1} and \textit{sel}, and \textit{out1} is its output. The word \textit{u0} in line 7 is a user defined label or identifier. Note that when using logic gate models, the output of the logic gate is the first parameter, while the rest are treated as inputs. In this way, logic gates with many inputs can be described easily. Line 8 describes the NOT gate (u2) connection, and line 9 the gate (U1). The outputs of gates U0 and U1 are fed as the inputs to OR gate (U3) where it is output is connected to MUX output port F. The order of appearance of the code does not affect the simulation results of this circuit because this is a model of a circuit schematic - this is one of the requirement of a HDL. 

Finally, line 11 shows the end of module description with the keyword \texttt{endmodule} followed by an optional colon and the module name. 

It is recommoded to save this file after the module name given in line 1 (\textit{mux2gate.sv} for this particular case). Only one module should be contained within one file, but multiple modules can reside within one file which is not recommonded. 

The general template for a \SV module is shown in Example \ref{lst:SVgenModel}. 
\begin{lstlisting}[language=SystemVerilog,caption=General \SV Module format,label={lst:SVgenModel}]
module <module_Name>(<port names>) ;
<port declarations> ;
<internal nets and variable, declarations> ;
<module function description> ;
endmodule
\end{lstlisting}
