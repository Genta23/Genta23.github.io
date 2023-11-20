---
title: What is admissible ??
layout: default
parent: Algorithm
grand_parent: Research
---
# A*とは

A*に関する説明です。簡易的にまとめたスライドなので、説明用スライドを作成中です。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSrrbRbB33mKSWZL0J3MULNkoZLV96u0dS8T6klotpXHpPXp0ZpruZ05Dy53-mvnSkp8Ip36LAEYPcZ/embed?start=false&loop=true&delayms=1000" frameborder="0" width="100%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

# admissibleについて

admissibleとは探索などで使用するヒューリスティック関数について使用される用語である。

## ヒューリスティック関数

まず、ヒューリスティックには必ずしも正しい答えを導けるとは限らないが、ある程度のレベルで正解に近い解を得ることができる方法という意味である。(wiki)

ヒューリスティック関数は評価関数の一種であり、現状態から目的状態までのコストの予測値を返すといった関数である。

## ヒューリスティック関数がadmissibleとは何か

現状態から目的状態までの実コストと、ヒューリスティック関数による予測コストを比較したときに、どのような状態でも予測コストが実コストを上回らないものが、admissible。

つまり、コストを予測するときに、過小評価することはあっても過大評価をすることはない、というのがadmissibleの条件。