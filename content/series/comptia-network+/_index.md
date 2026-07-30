---
title: "CompTIA Network+"
description: "Every post in the CompTIA Network+ (N10-009) series, in exam objective order — networking fundamentals through to troubleshooting, written as I actually studied each one."
showTableOfContents: false
showHero: true
heroStyle: "background"
layoutBackgroundHeaderSpace: true
---

{{< lead >}}
The full CompTIA Network+ series. I went through each objective from Domain 1 to Domain 5 and wrote a post about it, roughly in that order. Each one is grounded in something real wherever I could manage it, rather than just definitions off a list.
{{< /lead >}}

If you are studying the same certification, these are meant to be read in order below. A few other posts on the site cover related networking topics but sit outside this series — those are in the general [Latest]({{< ref "posts" >}}) list instead.

The five domains are not weighted equally on the actual exam. Troubleshooting and the foundational concepts domain carry the most weight between them, which is roughly why this series ended up as long as it did in those two areas specifically.

{{< chart >}}
type: 'bar',
data: {
  labels: ['1.0 Networking Concepts', '2.0 Network Implementation', '3.0 Network Operations', '4.0 Network Security', '5.0 Network Troubleshooting'],
  datasets: [{
    label: '% of Exam',
    data: [23, 20, 19, 14, 24],
  }]
},
options: {
  indexAxis: 'y',
  plugins: {
    legend: {
      display: false
    }
  },
  scales: {
    x: {
      title: {
        display: true,
        text: '% of Exam'
      }
    }
  }
}
{{< /chart >}}

---
