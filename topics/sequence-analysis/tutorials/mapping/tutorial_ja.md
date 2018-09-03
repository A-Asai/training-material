---
layout: tutorial_hands_on
topic_name: sequence-analysis
tutorial_name: mapping
---

# 次世代シークエンシングデータマッピングのイントロダクション
{:.no_toc}

実験の DNA/RNA リードをリファレンスゲノムにマッピングすることは現代のゲノムデータ解析の重要なステップです。マッピングによってリードはゲノムの特定の位置に割り当てられて遺伝子の発現レベルが分かるような洞察が得られます。ここではマッパーである 'Bowtie2' を使用してデータセットを処理をし、ソフトウェアである 'IGV' を使用してデータを視覚化します。


> ### Agenda
>
> このチュートリアルでは、以下のことを扱います:
>
> 1. TOC
> {:toc}
>
{: .agenda}

# マッピング 
> ### {% icon hands_on %} ハンズオン: Bowtie2 でマッピングを行う
>
> Bowtie2 で Galaxy 内のデータを処理して結果を見ていきます。 
>
> 1. ['GSM461178_untreat_paired_subset_1.fastq'](https://zenodo.org/record/61771/files/GSM461178_untreat_paired_subset_1.fastq) データセットを [Zenodo](https://zenodo.org/record/61771) から Galaxy にロードする。
>    
>    > ### {% icon tip %} Tip: リンクからデータをインポートする 
>    >
>    > * リンクをコピーする [Zenodo で右クリック -> リンクをコピー]
>    > * Galaxy Upload Manager を開く
>    > * **Paste/Fetch Data** を選択する
>    > * テキストボックスにリンクをペーストする 
>    > * **Start** を押す   
>    {: .tip}
>
>    > ### {% icon tip %} Tip: データファイルがヒストリーにインポートされたらファイルタイプを `fastq` から `fastqsanger` に変更する 
>    >
>    > * ヒストリーのデータセットに表示されている鉛筆ボタンをクリックする 
>    > * トップにある **Datatype** を選択する 
>    > * `fastqsanger` を選択する
>    > * **save** を押す
>    {: .tip}
>
> 2. **Bowtie2** {% icon tool %}: 左側のツールバーからマッパーの 'bowtie2' を検索してデータセットでマッパーを実行する。
>
>    > ### {% icon tip %} Tip: ツールの検索 
>    >
>    > * 左側にある検索画面をクリックする 
>    > * **bowtie2** とタイプする
>    > * **bowtie2** を選択する
>    > * アップロードされたデータセットを fastq ファイルとして選択する 
>    > * リファレンスゲノムとして human hg19 を選択する
>    {: .tip}
>
> 3. 右側のヒストリーパネルにある Bowtie2 のアウトプットをクリックします。ここに書いてある情報をよく見てください:
>    
>           100000 reads; of these: 100000 (100.00%) were unpaired;
>           of these: 99407 (99.41%) aligned 0 times
>           149 (0.15%) aligned exactly 1 time
>           444 (0.44%) aligned >1 times
>           0.59% overall alignment rate
>
>
>    > ### {% icon question %} Questions
>    >
>    > - ここにはどんな情報がありますか？
>    > - すべて期待通りのものですか？ 
>    > - なぜこのマッピングはうまくいってないのでしょうか？ 
>    >
>    >    <details>
>    >    <summary>クリックして回答を表示</summary>
>    >    <ol type="1">
>    >    <li>ここにある情報は量的なものです。いくつの配列がアラインメントされているか見ることができます。ここには質的な情報については記載されておりません。</li>
>    >    <li>いいえ、全てのリードの 0.59% しかマッピングできませんでした。</li>
>    >    <li>私たちは誤ったリファレンスゲノムに対してマッピングしていたのです！ </li>
>    >    </ol>
>    >    </details>
>    {: .question}
>
>
> 10. **Bowtie2** {% icon tool %}: 正しいリファレンスゲノムである 'Drosophila melanogaster' または略称である 'dm3' で Bowtie2 を再実行する。
>
>       > ### {% icon comment %} Comments
>       > - 分かりやすいデータセットの名前を付けることをお勧めします。
>       {: .comment}
>
{: .hands_on}

# 視覚化 

IGV browser のユーザーインターフェースの一般的な説明については以下で読むことができます: [IGV Browser description]({{site.baseurl}}/topics/introduction/tutorials/igv-introduction/tutorial.html)

> ### {% icon hands_on %} ハンズオン: IGV browser で視覚化する
>
>Integrative Genomics Viewer (IGV) は大規模な、統合ゲノムデータセットでのインタラクティブな調査のための高性能視覚化ツールです。これは配列ベースや次世代シークエンスデータ、そしてゲノムアノテーションを含む様々なデータタイプに対応しています。以下では計算されたマッピングを視覚化するためにこれを使用します。 
>
> 1. **IGV** {% icon tool %}: 結果を IGV で表示するためにコンピュータのローカルにある IGV browser を開く。 
> 2. **Galaxy** {% icon tool %}: 右側のヒストリーパネルにある Bowtie2 のアウトプットをクリックする。  
> 3. **Galaxy** {% icon tool %}: 計算された Bowtie2 の結果のヒストリーを選択して 'display with IGV' の 'local' をクリックする。 
> 4. **IGV** {% icon tool %}: BAM ファイルは IGV browser で開き、ゲノムは自動的に読み込まれます。
>
>       > ### {% icon tip %} Tip: より多くのゲノムにアクセスする 
>       >
>       >If the genome of your interest is not there check if its
>       >available via "More...". Is this is not the case you can add it manually via the menu
>       >"Genomes -> Load Genome from..."
>       >
>       > ![alt text](../../images/igv_select_genome.png "Select genome")
>       {: .tip}
> 5. **IGV** {% icon tool %}: 表示したい部分は第4染色体、86,761 〜 87,907 の位置です。そこに移動します。 
> 6. **IGV** {% icon tool %}: 2つの views があります:
>       - アラインメントされたリードの view
>       - coverage の view
>
>
>       > ### {% icon question %} Questions
>       >
>       > - coverage view のバーが色付けされているのはどういう意味ですか？ 
>       > - リードが灰色の代わりに白色である理由は何でしょうか？ 
>       > - chr4:87482 の位置にマッピングされるリードの数はいくつでしょうか？
>       >
>       >    <details>
>       >    <summary>クリックして回答を表示</summary>
>       >    <ol type="1">
>       >    <li> ヌクレオチドの、クオリティで重み付けされたリードが20％以上リファレンス配列と異なる場合、IGV は各塩基のリードカウントに比例してバーを色付けします。</li>
>       >    <li>それらはマッピングのクオリティがゼロに等しいです。このマッピングクオリティの解釈はマッピングのアライナーに依存し、一般的に使用されるいくつかのアライナーはこのコンベンションを使用して複数のアラインメントでリードをマークします。そのような場合、リードは同様に良い配置で別の位置にもマッピングされます。それはまたリードが一意にはできない可能性もありますが他の位置には必ずしも同じような良いクオリティのヒットを与える訳ではありません。</li>
>       >    <li>7つのリードがあります。6つは正しい 'T' を、1つは 'G' のリードです。</li>
>       >    </ol>
>       >    </details>
>       {: .question}
>
{: .hands_on}

> ### {% icon hands_on %} ハンズオン: 良いマッピングと悪いマッピングの違い 
>
> 1. **Galaxy** {% icon tool %}: 良いデータセットと悪いデータセットの違いを示すために悪いマッピングのデータセットを追加で用意しました。['GSM461182_untreat_single_subset.fastq'](https://zenodo.org/record/61771/files/GSM461178_untreat_paired_subset_1.fastq) データセットを [Zenodo](https://zenodo.org/record/61771) から Galaxy へロードしてください。
>
>    > ### {% icon comment %} Comments
>    > If you are using the [Freiburg Galaxy instance](http://galaxy.uni-freiburg.de) を使用している場合は、このデータセットを 'Shared Data' -> 'Data Libraries' -> 'Galaxy Courses' -> 'RNA-Seq' -> 'fastq' でロードすることができます 
>    {: .comment}
>
> 2. **IGV** {% icon tool %}: 2つのマッピングを IGV にロードしてそれらを比較してみよう！ 
>
>       > ### {% icon question %} Questions
>       >
>       > - IGV ではどのようにして2つ目のデータセットが悪いということが分かりますか？ 
>       >
>       >    <details>
>       >    <summary>クリックして回答を表示</summary>
>       >    <ol type="1">
>       >    <li>白色/透明のリードはマッピングのクオリティが悪いことを示しています。次に、リファレンスゲノムが異なる場合 IGV はヌクレオチドを色で示します。</li>
>       >    </ol>
>       >    </details>
>       {: .question}
>
{: .hands_on}
