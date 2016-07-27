---
layout: post
title: Finish migrating Objenesis to GitHub
date: '2015-05-17T20:05:00.001-07:00'
author: Henri Tremblay
tags:
- What I learned today
- Projects
modified_time: '2015-05-18T12:58:03.949-07:00'
blogger_id: tag:blogger.com,1999:blog-8282654404214414992.post-8775453809402232693
blogger_orig_url: http://blog.tremblay.pro/2015/05/finish-migrating-objenesis-to-github.html
---

<div dir="ltr" style="text-align: left;" trbidi="on">With <a href="http://www.codehaus.org/" rel="nofollow"
                                                             target="_blank">Codehaus</a> and <a
  href="http://google-opensource.blogspot.ca/2015/03/farewell-to-google-code.html" rel="nofollow" target="_blank">Google
  Code</a>&nbsp;closing, I'm happily required to migrate <a href="http://objenesis.org/" rel="nofollow" target="_blank">Objenesis</a>
  and <a href="http://easymock.org/" rel="nofollow" target="_blank">EasyMock</a>. Both projects have an hybrid hosting
  using different platforms. I've decided to start with Objenesis which is easier to migrate. I'll describe the process
  in this post. Hopefully, it will be helpful to someone but, in fact, I'm also looking forward to your feedback to see
  if I can improve my final setup.<br/><br/><a name='more'></a><br/><br/>So, Objenesis source code was originally on
  Google Code. I've moved it to GitHub some years ago mainly because pull requests are awesome. The website is also a
  GitHub page. Part of the documentation is on Google Code wiki, the binaries are on Google Code as well as the issue
  tracker.<br/><br/>Google being nice folks, they are providing a <a href="https://code.google.com/export-to-github"
                                                                     rel="nofollow" target="_blank">nice way</a> to
  migrate everything to GitHub. But I couldn't use that because<br/>
  <ul style="text-align: left;">
    <li>I don't want the project to end up in <span style="font-family: Courier New, Courier, monospace;">henri-tremblay/objenesis</span>.
      I want it where it is now, in the <a href="https://github.com/easymock" rel="nofollow" target="_blank">EasyMock
        organisation</a></li>
    <li>I only want to migrate the wiki and the issues since the sources are already there</li>
  </ul>
  <div>So here's what I did instead.</div>
  <br/>
  <h2 style="text-align: left;"></h2>
  <h2 style="text-align: left;">Issue tracker</h2>The issue tracker was migrated using the <a
    href="https://code.google.com/p/support-tools/wiki/IssueExporterTool" rel="nofollow" target="_blank">IssueExporterTool</a>.
  It worked perfectly (but is a bit slow as advertised).<br/>
  <h2 style="text-align: left;"></h2>
  <h2 style="text-align: left;">Wiki pages</h2>At first, I tried to export the entire Objenesis project to Github to be
  able to retrieve the wiki pages and then move them to the real source code. The result was quite bad because the
  tables are not rendered correctly. So I ended up manually migrating each page to markdown. There was only 3 pages so
  it wasn't too bad.<br/>
  <h2 style="text-align: left;"></h2>
  <h2 style="text-align: left;">Binaries</h2>This was more complicated. Maven binaries are already deployed through the
  Sonatype Nexus. But I needed to migrate the standalone bundles. I've looked into three options:<br/>
  <ul style="text-align: left;">
    <li>GitHub releases</li>
    <li>Bintray</li>
    <li>Both (GitHub releases exported to Bintray)</li>
  </ul>
  <br/>GitHub releases are created automatically when you tag in git. But you also seem to be able to add some release
  notes and binaries over that. I didn't want to dig too much into it. I knew I wanted to be in Bintray in the end. And
  I wanted easy automation. Bintray was easy to use so I went for Bintray only. Be aware, the way I did the migration is
  low-tech (but works).<br/>
  <ol style="text-align: left;">
    <li>Make a list of all the binaries to migrate</li>
    <li>Get them with <span style="font-family: Courier New, Courier, monospace;">wget https://objenesis.googlecode.com/files/objenesis-xxx.zip</span>
    </li>
    <li>Create an <a href="https://bintray.com/easymock" rel="nofollow" target="_blank">organisation</a>, a <a
      href="https://bintray.com/easymock/distributions" rel="nofollow" target="_blank">distribution repository</a> and
      an <a href="https://bintray.com/easymock/distributions/objenesis" rel="nofollow" target="_blank">objenesis
        package</a> in Bintray
    </li>
    <li>Add my GPG key to my Bintray account</li>
    <li>Upload everything using REST (<span style="font-family: Courier New, Courier, monospace;">curl -T objenesis-xxx.zip -H "X-GPG-PASSPHRASE: ${PASSPHRASE}" -uhenri-tremblay:${API_KEY} https://api.bintray.com/content/easymock/distributions/objenesis/xxx/objenesis-xxx-bin.zip?publish=1</span><span
      style="font-family: inherit;">)</span></li>
  </ol>
  <br/>It works, the only drawback is that no release notes are provided. They've always been on the <a
    href="http://objenesis.org/notes.html" rel="nofollow" target="_blank">website</a>.<br/>
  <h2 style="text-align: left;"></h2>
  <h2 style="text-align: left;">Project moved</h2>Finally, I've set the "Project Moved" flag to target <a
    href="https://github.com/easymock/objenesis" rel="nofollow" target="_blank">GitHub</a>. This quite aggressively
  redirects you to GitHub if you try to access <a href="https://code.google.com/p/objenesis">https://code.google.com/p/objenesis</a>.<br/>
  <div><br/></div>
</div>