# how-can-we-understand-mircobiome
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import seaborn as sns
import scipy
from scipy import stats
%matplotlib inline
#family_genus
#alpha diversity
#rank sum test & box plot
genus_abundance_df = pd.read_csv('/Users/sun/Desktop/summer/genusAbd.csv',index_col=0)
otu_abundance_df = pd.read_csv('/Users/sun/Desktop/summer/otuAbd.csv',index_col=0)
genus_count_df = pd.read_csv('/Users/sun/Desktop/summer/genusCount.csv',index_col=0)
otu_count_df = pd.read_csv('/Users/sun/Desktop/summer/otuCount.csv',index_col=0)
#alpha diversity data
genus_alpha_df = pd.read_csv('/Users/sun/Desktop/summer/genusAlpha.csv',index_col=0)
del genus_alpha_df['Observed OTU']
otu_alpha_df = pd.read_csv('/Users/sun/Desktop/summer/otuAlpha.csv',index_col=0)
del otu_alpha_df['Observed OTU']
def Alpha_diveristy_analysis(alpha_df):
    #final p value
    healthy=alpha_df[:alpha_df.shape[0]/2].values
    sick=alpha_df[alpha_df.shape[0]/2 :].values
    z_stat, p_val=scipy.stats.ranksums(healthy, sick)
    #reshape alpha diversity dataframe for plotting
    healthy_df=pd.DataFrame(data=healthy, index=None, columns=['B'])
    sick_df=pd.DataFrame(data=sick, index=None, columns=['I'])
    alpha_df_reshape=pd.concat([healthy_df, sick_df], axis=1)
    #plot alpha diverisity
    alpha_plot=sns.boxplot(alpha_df_reshape)
    alpha_plot.set(xlabel='status', ylabel='Shannon')
    plt.show()
    print 'Alpha Diversity p value:',p_val
Alpha_diveristy_analysis(genus_alpha_df)
Alpha_diveristy_analysis(otu_alpha_df)
