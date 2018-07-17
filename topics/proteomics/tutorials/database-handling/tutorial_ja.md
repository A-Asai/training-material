---
layout: tutorial_hands_on
topic_name: proteomics
tutorial_name: database-handling
---

# イントロダクション
{:.no_toc}

質量分析に基づくプロテオミクスの研究において、ペプチドは、peptide-spectral matching と呼ばれるタンデム質量分析（MS2）によって割り当てられます。ペプチドスペクトルマッチングは MS2 スペクトルを理論スペクトルに一致させるために検索アルゴリズムを用いることで一般的に行われます。理論スペクトルは FASTA データベースにあるタンパク質の消化についての理論上のデータから生成されています。理想的には、タンパク質 FASTA データベースには調査している生物のすべてのタンパク質が含まれています。

> ### Agenda
>
> このチュートリアルでは、以下を扱います:
>
> 1. TOC
> {:toc}
>
{: .agenda}


# タンパク質検索データベースのアップロード

タンパク質検索データベース（タンパク質配列を含む FASTA ファイル）はいろいろな方法でアップロードできます。次の3つの方法があります:

*   **Protein Database Downloader** {% icon tool %} を使用する。
*   データベースへの web リンクを直接使用する。
*   データライブラリからデータベースをアップロードする。

このチュートリアルでは、タンパク質検索データベースを生成するための **Protein Database Downloader** {% icon tool %} について説明します。このために必要な生物のプロテオームをダウンロードします。このチュートリアルでは、ヒトのプロテオームのデータベースを使用します。

> ### {% icon hands_on %} ハンズオン: タンパク質検索データベースをアップロードする 
>
> 1. このデータベース操作チュートリアル用の新しいヒストリーを作成する。
> 2. **Protein Database Downloader** {% icon tool %} を開く
> 3. ドロップダウンメニューから `Taxonomy`: "Homo sapiens (Human)" と `reviewed`: "UniprotKB/Swiss-Prot (reviewed only)" を選択する。
> 4. `Execute` をクリックする。これで、あなたのヒストリーに `Protein database` という名前の新しいデータセットがあるはずです。
> 5. `Protein database` を `Main database` に名前を変更する。
>
>  > ### {% icon tip %} Tip: uniprot データベースの種類
>  > Uniprot にはいくつかの種類のデータベースがあります。レビューされた (UniProtKB/Swissprot) データベースのみをダウンロードするか、レビューされていない (UniProtKB/TREMBL) データベースのみ、または両方  (UniProtKB) のデータベースをダウンロードするかを選択することができます。よく研究されているモデル生物、例えばヒトやショウジョウバエといった生物のレビューされた (Swissprot) データベースには精選されたタンパク質が含まれていてより小さなデータベースでよりクリーンな検索結果を得られるかもしれません。しかし、レビューされていないタンパク質に興味がある場合は、TrEMBL データベースを含めるのが賢明かもしれません。
>  >
>  > またチェックボックス `Include isoform data` を `Yes` に設定することでタンパク質アイソフォームを含めることができます。
>  {: .tip}
>
>  > ### {% icon question %} Question
>  > "reference proteome set" と "complete proteome set" の違いはなんですか?
>  >
>  > > ### {% icon solution %} Solution
>  > > 1.  UniProt の complete proteome はゲノムが完全にシークエンスされた生物で発現されると考えられるタンパク質のセットからなります。Reference proteome は代表的な、よく研究されたモデル生物または生物医学や生物工学の研究の対象となる生物の完全なプロテオームです。Reference proteomes は UniProtKB 内で見られる分類学的な多様性の代表的な cross-section を構成します。特定の重要な種は特定の生態系または関心のある種族の多数のリファレンスプロテオームによって表現できる可能性があります。[Link to source](http://www.uniprot.org/help/reference_proteome)
>  > {: .solution }
>  {: .question}
{: .hands_on}


# コンタミネーションデータベース

プロテオームのサンプルでは、いくつかのタンパク質混入物が一般的に存在します。これらのタンパク質混入物はサンプル調製中にサンプル中にコンタミします。これらのコンタミネーションの原因は研究者またはコンタミした細胞培地によるものであると考えられます。混入物由来のスペクトルを誤って特定することを避けるために、一般的な研究室でコンタミするタンパク質配列をデータベースに追加します。これには2つの利点があります:
1. コンタミネーションを観測することができ、重度にコンタミしたサンプルは解析から除外することができます。
2. 混入物のペプチドをデータベース中の類似のペプチドに誤って割り当てることがなくなり誤った陽性結果を特定するリスクを減少します。

コンタミネーション用に一般的に使用されるデータベースは **c**ommon **R**epository of **A**dventitious **P**roteins (cRAP) です。細胞培養で生成されたサンプルを使用する場合は、_Mycoplasma_ のプロテオームを検索データベースに含めることを強く推奨します。_Mycoplasma_ 感染は細胞培養において非常に一般的なものでありしばしば見落とします ([Drexler and Uphoff, Cytotechnology, 2002](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3463982/))。

> ### {% icon hands_on %} ハンズオン: コンタミネーションデータベース
> 1. **Protein Database Downloader** {% icon tool %} を開く。
> 2. `Download from`: "cRAP (contaminants)" を選択し実行する。
> 3. 新しいデータベースを "crap database" という名前に変更する。
>
> 目的のタンパク質から混入物を区別するには、それぞれのコンタミネーションタンパク質にタグを追加する必要があります。 
>
> 1. Run **FASTA-to-Tabular** {% icon tool %} on your crap database.
> 2. 新たなアウトプットで **Add column** {% icon tool %} を実行する。`Add this value` の欄に "CONTAMINANT" を入力して実行する。
> 3. **Tabular-to-FASTA** {% icon tool %} を実行する。1列目と3列目をタイトル列として、2列目を配列の列として使用する。
> 4. **Tabular-to-FASTA** {% icon tool %} のアウトプットを "Tagged cRAP database" に名前を変更する。
>
>
>  > ### {% icon question %} Question
>  > 1. cRAP データベースにはいくつかのヒトタンパク質が含まれています。ヒトサンプル中の典型的な混入物を特定する場合それはどういう意味でしょうか？
>  > 2. 非ヒトサンプルではどういう意味でしょうか？
>  >
>  > > ### {% icon solution %} Solution
>  > > 1. ヒト由来のサンプルでは、特定されたヒト由来の混入物は必ずしも混入サンプルを意味するものではありません。このタンパク質は研究調査サンプルに由来するもののように扱ってもよいです。使用者はデータを解釈する際によく考えて判断することをお勧めします。
>  > > 2. 非ヒトサンプルでは、同定されたヒトの混入物は実験者によるコンタミネーションを意味しています。 
>  > {: .solution }
>  {: .question}
{: .hands_on}


> ### {% icon hands_on %} 任意のハンズオン: _Mycoplasma_ データベース
> 細胞培養におけるマイコプラズマ感染の 90 - 95 % は以下の種に由来します: _M. orale, M. hyorhinis, M. arginini, M. fermentans, M. hominis_ そして _A. laidlawii_ ([Drexler and Uphoff, Cytotechnology, 2002](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3463982/))。
>
> 1. **Protein Database Downloader** {% icon tool %} を使用して6種のマイコプラズマのデータベースをダウンロードする。これらはこのチュートリアルの次のパートでメインのデータベースにマージします。
> 2. **FASTA Merge Files and Filter Unique Sequences** {% icon tool %} を実行してすべてのマイコプラズマデータベースを1つに連結します。
> 3. Tag each entry in the combined database with the string "MYCOPLASMA_CONTAMINANT" by using **FASTA-to-Tabular** {% icon tool %}, **Add column** :wrench: and **Tabular-to-FASTA** :wrench:, as explained [above](#contaminant-databases).
> 4. **Tabular-to-FASTA** {% icon tool %} アウトプットを "Tagged Mycoplasma database" という名前に変更する。
>
>  > ### {% icon comment %} Comment
>  > レビューされたマイコプラズマデータベースにはすべての既知のタンパク質が含まれているわけではありません。TREMBL データベースも含めた方が良いです。_Mycoplasma_ プロテオームは比較的小さいので、TrEMBL 配列をダウンロードするだけではメインのデータベースのサイズはそれほど大きくならないでしょう。
>  {: .comment}
{: .hands_on}


# データベースをマージする 

使用する検索アルゴリズムによって、FASTA のすべてのエントリー（関心のあるタンパク質と混入物）を1つのデータベースにマージする必要があります。コンタミのデータベースはタグ付きのバージョンをマージしてください。

> ### {% icon hands_on %} ハンズオン: データベースをマージする
>
> 1. メインのデータベースとタグ付けした cRAP データベースで **FASTA Merge Files and Filter Unique Sequences** {% icon tool %} を実行する。
>
> 2. **任意**: マイコプラズマデータベースをマージする
>
>    このステップでは上でダウンロードしたマイコプラズマタンパク質データベースをマージすることもできます。**FASTA Merge Files and Filter Unique Sequences** {% icon tool %} にインプットを追加するだけです。`Insert Input FASTA file(s)` をクリックすると任意の数のタンパク質でデータベースを追加することができます。
{: .hands_on}


# デコイデータベースを作成する

ペプチドおよびタンパク質の False Discovery Rate（FDR）計算の最も一般的な方法はサンプル中に存在しないことが予想されるタンパク質配列を加えることです。これらは、デコイタンパク質配列とも呼ばれています。これは、標的タンパク質エントリーの逆配列を生成し、これらのタンパク質エントリーをタンパク質データベースに付加することによって行うことができます。Some search algoritmms use premade target-decoy protein sequences while others can generate a target-decoy protein sequence database from a target protein sequence database before using them for peptide spectral matching.

> ### {% icon hands_on %} ハンズオン: デコイデータベースを作成する
> 1. マージしたデータベースで **DecoyDatabase**  {% icon tool %} を実行する。
> 2. flag (-append) を `Yes` に設定して実行する。
>
>  > ### {% icon tip %} Tip: Decoy tags
>  > デコイのタグとして入力した文字列は各デコイタンパク質のエントリーの説明に接頭辞または接尾辞（選択できます）として追加されます。こうしてデコイが計算された標的データベース内のエントリーを確認することができます。
>  {: .tip}
>
>  > ### {% icon comment %} Comment
>  > **DecoyDatabase**  {% icon tool %} はいくつかのデータベースをインプットとして自動的に1つのデータベースにマージします。
>  {: .comment}
{: .hands_on}


# 結論
{:.no_toc}

タンパク質データベースを最新の状態に保つには、ハンズオンセクションからワークフローを作成することをお勧めします（ワークフローについては[このチュートリアル]({{site.baseurl}}/topics/introduction/tutorials/galaxy-intro-101/tutorial.html)を参照してください）。またマイコプラズマデータベースを1つのファイルに結合して、それぞれのメインデータベースに簡単に加えることができます。

再現性の理由から最新のデータベースを使いたくないことがよくあります。そうであれば、このチュートリアルの巻末のデータベースを他のヒストリーに転送して操作してください。

最適なデータベースの構築についてさらに読むなら: ([Kumar et al., Methods in molecular biology, 2017](https://www.ncbi.nlm.nih.gov/pubmed/27975281))

このチュートリアルは GalaxyP-101 tutorial (https://usegalaxyp.readthedocs.io/en/latest/sections/galaxyp_101.html) の一部に基づいて作成されています。
