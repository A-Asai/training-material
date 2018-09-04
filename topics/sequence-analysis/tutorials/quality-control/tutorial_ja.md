---
layout: tutorial_hands_on
topic_name: sequence-analysis
tutorial_name: quality-control
---

# イントロダクション
{:.no_toc}

シークエンシング中に、曖昧なヌクレオチドの取り込みのような、エラーが導入される可能性があります。これは各シークエンシングプラットフォームの技術的な限界に起因しています。シークエンシングエラーは解析にバイアスをもたらし、最終的にはデータの誤解を招く可能性があります。 

したがってシークエンシングのクオリティチェックは生のシークエンスデータを受信した直後に行う不可欠なステップです。これはデータを取得するために使用されるシークエンシングプラットフォームに関係なく、適切な分析が確実に行われます。 

> ### Agenda
>
> このチュートリアルでは、以下のことを扱います:
>
> 1. TOC
> {:toc}
>
{: .agenda}

# シークエンスデータをインポートする 

> ### {% icon hands_on %} ハンズオン: データのアップロード 
>
> 1. ヒストリーを新規作成する 
> 2. 次の FASTQ ファイルをインポートする: [`GSM461178_untreat_paired_subset_1`](https://zenodo.org/record/61771/files/GSM461178_untreat_paired_subset_1.fastq)
>
>    > ### {% icon tip %} Tip: リンクからデータをインポートする 
>    >
>    > * リンクをコピーする 
>    > * Galaxy Upload Manager を開く
>    > * **Paste/Fetch Data** を選択する
>    > * テキストボックスにリンクをペーストする 
>    > * **Start** を押す    
>    {: .tip}
>
>    > ### {% icon tip %} Tip: データファイルがヒストリーにインポートされたらファイルタイプを `fastq` から `fastqsanger` に変更する 
>    >
>    > * ヒストリーのデータファイルに表示されている鉛筆ボタンをクリックする 
>    > * トップにある **Datatype** を選択する 
>    > * `fastqsanger` を選択する
>    > * **save** を押す
>    {: .tip}
>
>    > ### {% icon comment %} Comments
>    >
>    > データセットの名前を "First dataset" に変えましょう
>    {: .comment}
> デフォルトでは、リンクからデータをインポートすると、Galaxy での名前はその URL になります。
{: .hands_on}

# クオリティチェック 

配列のクオリティや生データをさらにフィルタリングする方法を推定するために、異なるインジケータをチェックすることができます:

- 配列のクオリティスコアについて
    - 塩基ごとの配列のクオリティ 
    - 配列ごとのクオリティスコア 
    - タイルごとの配列のクオリティ 
- 配列の含有量について 
    - 塩基ごとの配列の含有量 
    - 配列ごとの GC 含有量
    - 塩基ごとの N 含有量 
- 配列長の分布での配列長 
- 重複配列 
- タグ配列について 
    - アダプターのコンタミネーション 
    - K-mer の含有量 

[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) はハイスループットシークエンシングパイプラインからの生配列データをクオリティコントロールする簡単な方法を提供するオープンソースのツールです。それは低いクオリティスコアのリードを取り除き、どのデータが解析のバイアスの原因となるかについての簡単な概要を提供するグラフィックスおよび推定を生成する。 

> ### {% icon hands_on %} ハンズオン: クオリティチェック 
>
> 1. **FastQC** {% icon tool %}: インポートした FastQ ファイルで、デフォルトの設定で **FastQC Read Quality reports** を実行する 
> 2. webpage のアウトプットで FastQC report を検査する 
>
>    > ### {% icon tip %} Tip: Galaxy でファイルの内容を検査する 
>    >
>    > * ヒストリーのファイル名の右にある目 ("データを表示する") をクリックする 
>    > * 中央でファイルの内容を検査する 
>    {: .tip}
>
>    > ### {% icon question %} Questions
>    >
>    > 1. クオリティスコアはどれくらいが良いですか？ 
>    > 2. なぜ塩基ごとの配列の含有量と配列ごとの GC 含有量のグラフが警告されているのですか？ 
>    > 3. シークエンスを改善するためには何が必要ですか？ 
>    >
>    >    <details>
>    >    <summary>クリックして回答を表示</summary>
>    >    <ol type="1">
>    >    <li>シークエンスのスコアは非常に良いです: たとえリードの末端でクオリティがわずかに低下しても、FastQC は警告しません</li>
>    >    <li>シークエンスの始めは、塩基ごとの配列の含有量は実際には良くなくパーセンテージは等しくありません。GC 含有量については、分布は左にわずかにシフトし、高すぎます。</li>
>    >    <li>配列の末端を少しだけトリミングすることができますが、配列がすでに小さいのであまり大きくはトリミングできません。</li>
>    >    </ol>
>    >    </details>
>    {: .question}
{: .hands_on}

# 配列のクオリティを改善する 

クオリティのグラフによって提供される情報に基づいて、下流の解析におけるバイアスを避けるために配列を処理しなければなりません。 

一般的に、クオリティの処理を行う場合は次の通りです:

- 配列のフィルタリングする 
    - 平均してクオリティスコアが低い 
    - 小さすぎる 
    - N 塩基が多すぎる
    - GC 含有量に基づいて 
    - ...
- 配列をカット/トリミングする
    - クオリティスコアが低い部分 
    - 末端 
    - ...

配列全体のクオリティを向上させるために、[Trim Galore!](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) ツールを使用します。このツールはアダプターのトリミングとクオリティコントロールを自動化することにより配列のクオリティを向上させます。 

> ### {% icon hands_on %} ハンズオン: 配列のクオリティを改善する 
>
> 1. **Trim Galore!** {% icon tool %}: インポートした FastQ ファイルに対して **Trim Galore! Quality and adapter trimmer of reads** を実行する
>
>    > ### {% icon question %} Questions
>    >
>    > Which parameters must be applied to follow the previous recommendations?
>    >
>    > <details>
>    > <summary>クリックして回答を表示</summary>
>    > デフォルトのまま使用します:
>    > <ul>
>    > <li>​
ライブラリー作成中に使用されたアダプター配列が分かっている場合は、その配列を記入してください。それ以外の場合はアダプター配列の自動検出とトリミングのオプションを使用しましょう。</li>
>    > <li>Trimming low-quality ends (below 20) from reads in addition to adapter removal</li>
>    > <li>Option for required overlap (in bp) with adapter sequence can be tweaked. The default value "1" is too stringent, and on average 25% of reads will be trimmed. Please set it to 5 bases to loose the required overlap</li>
>    > <li>Removing reads shorter than 20 bp</li>
>    > </ul>
>    > </details>
>    {: .question}
>
> 2. **FastQC** {% icon tool %}: Re-run **FastQC Read Quality reports** on the quality controlled data, and inspect the new FastQC report
>
>    > ### {% icon question %} Questions
>    >
>    > 1. How many sequences have been removed?
>    > 2. Has sequence quality been improved?
>    > 3. Can you explain why the per-base sequence content is not good now?
>    >
>    >    <details>
>    >    <summary>Click to view the answers</summary>
>    >    <ol type="1">
>    >    <li>Before Trim Galore, the dataset comprised 100,000 sequences. After Trim Galore, there are 99,653 sequences</li>
>    >    <li>The per-base quality score looks better, but other indicators show bad values now. The sequence length distribution is not clear anymore because sequences have different size after the trimming operation</li>
>    >    <li>The per-base sequence content has turned red. Again, the cause is the trimming of the end of some sequences</li>
>    >    </ol>
>    >    </details>
>    {: .question}
{: .hands_on}

The quality of the previous dataset was pretty good from beginning. The sequence quality treatment improved the quality score at the cost of other parameters.

# Impact of quality control

Now, we take a look at the impact of quality control and treatment on a bad dataset.

> ### {% icon hands_on %} Hands-on: Impact of quality control
>
> 1. Create a new history
> 2. Import the FASTQ file: [`GSM461182_untreat_single_subset`](https://zenodo.org/record/61771/files/GSM461182_untreat_single_subset.fastq)
> 3. **FastQC** {% icon tool %}: Run **FastQC Read Quality reports** on the newly imported dataset
>
>    > ### {% icon question %} Questions
>    >
>    > 1. How good is this dataset?
>    > 2. What needs to be done to improve the sequences?
>    >
>    >    <details>
>    >    <summary>Click to view the answers</summary>
>    >    <ol type="1">
>    >    <li>There is a red warning on the per-base sequence quality (pretty bad along the sequence but worse at the end of sequences), the per-base sequence content (bad at the beginning of the sequences), and the per-sequence GC content</li>
>    >    <li>The end of sequences must be cut.</li>
>    >    <li>Generally, the 5' end of each sequence read is not of bad quality unless something went wrong. Here, the problem is that the sample was sequenced using the an Illumina sequencing machine, which carries out its calibration while reading fragments that are in the beginning of the flowcell. Unfortunately, the first 100k reads which we selected for the analysis are generated during the calibration, a problem that we don't have with more recent sequencing machines. However, if you adopted one of the latest sequencing machine and still experience bad quality bases at the beginning of the reads, please don't just trim them, but consider investigating the problem further</li>
>    >    </ol>
>    >    </details>
>    {: .question}
>
> 4. **Trim Galore** {% icon tool %}: Run Run **Trim Galore! Quality and adapter trimmer of reads** on the new dataset to apply the decisions taken at the previous step
> 5. **FastQC** {% icon tool %}: Re-run **FastQC Read Quality reports** to check the impact of Trim Galore
>
>    > ### {% icon question %} Questions
>    >
>    > 1. How many sequences have been removed?
>    > 2. Has sequence quality been improved?
>    > 3. Can you explain why the per-base sequence content is not good now?
>    >
>    >    <details>
>    >    <summary>Click to view the answers</summary>
>    >    <ol type="1">
>    >    <li>Before Trim Galore the dataset comprised 100,000 sequences. After Trim Galore, there are 97,644 sequences</li>
>    >    <li>The per-base quality score looks better (not red anymore), but the per-base sequence content, even if slightly better, is still red</li>
>    >    </ol>
>    >    </details>
>    {: .question}
{: .hands_on}

# Conclusion
{:.no_toc}

In this tutorial we checked the quality of two datasets to ensure that their data looks good before inferring any further information. This step is the baseline for any pipeline analysis such as RNA-Seq, ChIP-Seq, or any other OMIC analysis relying on NGS data. Quality control steps are similar for any type of sequencing data:

![The quality control tutorial workflow](../../images/dive_into_qc_workflow.png)
