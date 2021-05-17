# ccne:Carbapenemase-encoding gene Copy Number Estimator
Carbapenemase-encoding gene Copy Number Estimator (ccne) is a tool to estimate the copy number of carbapenemase-encoding gene. It uses housekeeping gene as the reference and compares the count of reads that mapped to carbapenemase-encoding genes with the count of reads that mapped to the reference gene. 
***
# Usage

Name:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ccne 1.0.0 by Jianping Jiang<br/>
Synopsis:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Carbapenemase-encoding gene copy number estimator<br/>
Usage:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ccne --carb KPC-2 --sp Kp --in File.list --out result.txt<br/>
General:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--help&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This&nbsp;help<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--version&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Print&nbsp;version&nbsp;and&nbsp;exit<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--quiet&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;No&nbsp;screen&nbsp;output&nbsp;(default&nbsp;OFF)<br/>
Setup:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--dbdir&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CCNE&nbsp;database&nbsp;root&nbsp;folders&nbsp;(default&nbsp;'/ccne/data')<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--listdb&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&nbsp;all&nbsp;configured&nbsp;databases<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--listsp&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&nbsp;all&nbsp;configured&nbsp;species&nbsp;and&nbsp;housekeeping&nbsp;genes<br/>
Input:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--carb&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Carbapenemase-encoding&nbsp;gene&nbsp;name,&nbsp;such&nbsp;as&nbsp;KPC-2,&nbsp;NDM-1,&nbsp;etc.&nbsp;Please&nbsp;refer&nbsp;to&nbsp;--listdb&nbsp;(required)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--sp&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Species&nbsp;name[Kp|Ec|Ab|Pa]&nbsp;(required)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--ref&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reference&nbsp;gene&nbsp;defalut(Kp:rpoB&nbsp;Ab:rpoB&nbsp;Ec:polB&nbsp;Pa:ppsA),&nbsp;please&nbsp;refer&nbsp;to&nbsp;--listsp<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--in&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Input&nbsp;file&nbsp;name&nbsp;(required)<br/>
Outputs:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--out&nbsp;[X]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Output&nbsp;file&nbsp;name&nbsp;(required)<br/>
Computation:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--flank&nbsp;[N]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The&nbsp;flanking&nbsp;length&nbsp;of&nbsp;sequence&nbsp;to&nbsp;be&nbsp;excluded&nbsp;(default&nbsp;'100')<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--cpus&nbsp;[N]&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Number&nbsp;of&nbsp;CPUs&nbsp;to&nbsp;use&nbsp;(default&nbsp;'1')<br/>
***
# Licence
* GPL V3

# Author
* Jiang Jianping
* Institute of Antibiotics, Huashan hospital, Fudan University
* jiangjianping@fudan.edu.cn
