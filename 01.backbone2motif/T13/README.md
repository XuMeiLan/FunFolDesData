

```python
# base libraries
import warnings; warnings.simplefilter('ignore')
import os
import sys

# external libraries
import matplotlib
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
from IPython.display import display_javascript, display_html, display

# local libraries
sys.path.append(os.path.abspath("../"))
import readme_utils

matplotlib.rcParams['svg.fonttype'] = 'none'
pd.set_option('display.max_columns', None)
sns.set_style("whitegrid")
sns.set_palette('deep')
```

# Load baseline data for the Benchmark


```python
info = readme_utils.io.load_benchmark_info("_info.yaml")
readme_utils.plot.show_info(info)
```


<div style="width:100%;">
    <table style="width:60%;margin-left:20%">
        <tr style="border-top: 3px solid black;border-bottom: 1px solid black;">
            <th colspan="6" style="text-align:center;" >Benchmark</th>
        </tr>
        <tr>
            <th colspan="2" style="text-align:center;" >ID</th>
            <th colspan="2" style="text-align:center;" >CATH</th>
            <th colspan="2" style="text-align:center;" ># motifs</th>
        </tr>
        <tr>
            <td colspan="2" style="text-align:center;" >T13</td>
            <td colspan="2" style="text-align:center;" >CATH.2.40.40.20</td>
            <td colspan="2" style="text-align:center;" >2</td>
        </tr>
        <tr style="border-top: 3px solid black;border-bottom: 1px solid black;">
            <th colspan="6" style="text-align:center;" >Structures</th>
        </tr>
        <tr>
            <th colspan="3" style="text-align:center;" >Query</th>
            <th colspan="3" style="text-align:center;" >Reference</th>
        </tr>
        <tr>
            <td colspan="3" style="text-align:center;" >2pjhB.pdb</td>
            <td colspan="3" style="text-align:center;" >1cr5B.pdb</td>
        </tr>
        <tr style="border-top: 3px solid black;border-bottom: 1px solid black;">
            <th colspan="6" style="text-align:center;" >Design</th>
        </tr>
        <tr>
            <th colspan="2" style="text-align:center;" >motif</th>
            <th colspan="2" style="text-align:center;" >chain</th>
            <th colspan="2" style="text-align:center;" >sequence shift</th>
        </tr>
        <tr>
            <td colspan="2" style="text-align:center;" >119-142,168-173</td>
            <td colspan="2" style="text-align:center;" >B</td>
            <td colspan="2" style="text-align:center;" >108</td>
        </tr>
        <tr style="border-top: 3px solid black;">
            <th colspan="2" style="text-align:center;border-right: 1px solid black;" >Experiments</th>
            <td colspan="2" style="text-align:center;" >abinitio</td>
            <td colspan="2" style="text-align:center;" >nubinitio</td>
        </tr>
        <tr style="border-top: 3px solid black;border-bottom: 1px solid black;">
            <th colspan="6" style="text-align:center;" >Fragment types</th>
        </tr>
        <tr>
            <th colspan="1" style="text-align:center;" >auto</th>
            <td colspan="5" style="text-align:left;" >Automatic fragment generation (sequence + secondary structure)</td>
        </tr>
        <tr>
            <th colspan="1" style="text-align:center;" >picker</th>
            <td colspan="5" style="text-align:left;" >Standard Rosetta fragment generation (sequence-based data)</td>
        </tr>
        <tr>
            <th colspan="1" style="text-align:center;" >wauto</th>
            <td colspan="5" style="text-align:left;" >Automatic fragment generation (secondary structure + angles + sasa)</td>
        </tr>
    </table>
    </div>



```python
base = readme_utils.io.load_baseline(info)
base
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>score</th>
      <th>lbl_CORE</th>
      <th>lbl_PICKED</th>
      <th>lbl_MOTIF</th>
      <th>lbl_QUERY</th>
      <th>sequence_B</th>
      <th>structure_B</th>
      <th>lbl_CONTACTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>199.185</td>
      <td>[B]</td>
      <td>[B]</td>
      <td>[B]</td>
      <td>[B]</td>
      <td>VKYGKRIHVLPIDDTVEGITGNLFEVYLKPYFLEAYRPIRKGDIFLVRGGMRAVEFKVVETDPSPYCIVAPDTVIHCEG</td>
      <td>LLELLLEEEEELHHHLLLLLLLLLLLLLLLLLLLLLLEEELLLEEEELLLLLLEEEEEEEELLLLEEELLLLLLEELLL</td>
      <td>1-11,36-60,67-79</td>
    </tr>
  </tbody>
</table>
</div>



# Fragment analysis


```python
fragments = readme_utils.io.load_fragments( info )
```


```python
matplotlib.rcParams.update({'font.size': 30})
readme_utils.plot.plot_fragments( fragments, info, base )
```


![png](README_files/README_6_0.png)



![png](README_files/README_6_1.png)


# Main data analysis


```python
df = readme_utils.io.load_main_data( info, base )
```


```python
print "columns:", ", ".join([str(x) for x in df.columns.values]), "\n"
df.groupby(["experiment", "fragments"]).count()["description"]
```

    columns: score, ALIGNRMSD, BUNS, B_ni_mtcontacts, B_ni_rmsd, B_ni_rmsd_threshold, B_ni_trials, COMPRRMSD, MOTIFRMSD, cav_vol, driftRMSD, finalRMSD, packstat, B_ni_rmsd_type, description, experiment, fragments, sequence_B, benchmark 
    





    experiment  fragments
    abinitio    auto         9939 
                picker       10293
                wauto        10296
    nubinitio   auto         10296
                picker       10296
                wauto        10296
    Name: description, dtype: int64



## Compare FFL vs. abinitio RMSD


```python
matplotlib.rcParams.update({'font.size': 30})
readme_utils.plot.plot_main_summary( df )
```


![png](README_files/README_11_0.png)



```python
matplotlib.rcParams.update({'font.size': 25})
readme_utils.plot.plot_main_distributions( df, 15 )
```


![png](README_files/README_12_0.png)


## FFL sequence retrieval


```python
matplotlib.rcParams.update({'font.size': 30})
readme_utils.plot.plot_aa_heatmaps( df, info, base, 0.1 )
```


![png](README_files/README_14_0.png)



```python
matplotlib.rcParams.update({'font.size': 30})
readme_utils.plot.plot_aa_similarities( df, info, base )
```


![png](README_files/README_15_0.png)


## HMM analysis
Check sequence recovery against the template's hmm.


```python
hmm = readme_utils.io.load_hmm_data( df, info )
matplotlib.rcParams.update({'font.size': 10})
sns.factorplot(x="fragments", y="count", col="experiment", order=["wauto", "picker", "auto"],
                data=hmm, kind="bar", size=4, aspect=.7);
plt.show()
```


![png](README_files/README_17_0.png)


## Success?
We measure success over the top 10% scored decoys of each experiment/fragment type; comparing the performance of FFL vs. that of _abinitio_.


```python
matplotlib.rcParams.update({'font.size': 30})
readme_utils.plot.check_success(df, info, base, 0.1, 3.0)
```


![png](README_files/README_19_0.png)
