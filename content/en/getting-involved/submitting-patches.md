---
title: "Submitting Patches"
description: "Instructions for submitting a patch to the Linux PTP Project."
---

### Submitting Patches

1. Before submitting patches, please make sure that you are starting
   your work on the **current HEAD** of the git repository.

2. Please checkout the [CODING_STYLE.org](https://github.com/richardcochran/linuxptp/blob/master/CODING_STYLE.org) file for guidelines on how to
   properly format your code.

3. Describe your changes. Each patch will be reviewed, and the reviewers
   need to understand why you did what you did.

4. **Sign-Off** each commit, so the changes can be properly attributed to
   you and you explicitly give your agreement for distribution under
   linuxptp's [license](/about/license/). Signing-off is as simple as:

   `git commit -s`

   or by adding the following line (replace your real name and email)
   to your patch:

   `Signed-off-by: Random J Developer <random@developer.example.org>`

5. Finally, send your patches via email to the [linuxptp-devel mailing
   list](https://lists.nwtime.org/sympa/info/linuxptp-devel), where they will be reviewed, and eventually be included in the
   official code base.

   `git send-email --to linuxptp-devel@lists.nwtime.org origin/master`

&nbsp; 
