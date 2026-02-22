+++
template = "post.html"

title = "tosca: a customizable and self-contained IoT framework"
description = """
`tosca` allows to design firmware for various hardware architecture and
the same time to create a controller able to interact with these firmware.
Its innovation resides in its security-by-design aspects, thus reducing the
amount of bugs at compile-time and the hazards concept that outlines clearly
the safety, economic, and privacy risks coming from the executions of
device operations.
"""
date = 2026-02-05
tags = ["rust", "secure-by-design", "tosca"]

[extra.languages]
message = "This post can also be read in "
translations = [
  { lang = "it", path = "/it/blog/nascita-di-tosca", name = "Italian" }
]
+++

The `tosca` framework finally came to light! 🎉

Starting as an academic project with the goal of creating an architecture that
might be more reliable in terms of usage and more transparent on the risks
about device operations. Afterwards, its entire conception has evolved during
the two years time into the idea of having a customizable and self-contained
framework.

With the customizable term, we mean that all the commands of a device
can be exposed such that a controller can interact with them and all of their
parameters. This property allows to map any device using `tosca` APIs.
With the self-contained term, we mean a framework that does not require external
libraries to build neither a firmware neither the associated controller.
Therefore, having something which does not
require having external dependencies to build an ecosystem.

At first, we are going to explain the `tosca` framework, its potentiality, but
also what it lacks, concluding with an example of APIs showcase.

# What is `tosca`?


# Main Objectives

# API Showcase

# What's missing?

# Conclusion
