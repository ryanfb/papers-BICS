5000-7000 words

Links
=====

* http://www.stoa.org/archives/1263
* http://www.clir.org/pubs/archives/Babeu2010.pdf

Abstract
========

The Son of Suda On-Line (SoSOL) represents the first steps towards a
collaborative, editorially-controlled, online editor for the Duke Databank of
Documentary Papyri (DDbDP). Funded by the Andrew W. Mellon Foundation’s
Integrating Digital Papyrology Phase 2 (IDP2), SoSOL provides a strongly
version-controlled front-end for editing and reviewing papyrological texts
marked up in EpiDoc XML. This is accomplished in a tagless environment through
the use of a dual-syntax grammar which provides a bidirectional unambiguous
mapping between EpiDoc and a plaintext Leiden-style markup dubbed Leiden+. For
version control, SoSOL uses the distributed version control system Git as its
backend. This allows us to essentially have a “forked” repository for each
user of the system while using very little space, yet still track change
history in a robust way so as to enable intelligent automatic merging of
submitted changes. While any user can edit anything, submitted changes must
pass through an editorial control workflow. Here editors can vote and comment
on the submission (as well as make editorial interventions) before it is
included in the public, “canonical” version of the repository. The process is
designed to maintain transparency and accurate attribution, with
post-submission editorial interventions appropriately attributed to the editor
who made them. The entire framework is implemented as a Ruby on Rails web
application, tested and deployed with JRuby so that it can run in any Java
Servlet Container such as Tomcat.

While development and documentation is still ongoing, in March of 2010 we
began to introduce papyrologists to using SoSOL at EpiDoc workshops in order
to gather feedback and make improvements. The results so far have been
encouraging, with over one thousand changes made to the DDbDP through SoSOL in
just four months. Previously, though the DDbDP was in electronic form, it was
very difficult for third parties to submit new texts to it or make emendations
and corrections to existing texts. With the integration, consolidation, and
EpiDoc standardization achieved under phase one of the Integrating Digital
Papyrology project laying the groundwork, SoSOL has been able to provide a
convenient interface and scholarly workflow for editing the DDbDP’s large
corpus of ancient documentary papyri (approximately 55,000 texts). We also
hope to extend the usefulness of this tool by making it a reusable open-source
software component. Effectively SoSOL will be the core component which manages
version control, users, and editorial workflow, while our project-specific
components for editing EpiDoc texts and aggregating disparate sources into
publications would become a separate piece of software which uses the SoSOL
component, called the Papyrological Editor. The core of the tool, which allows
anyone to edit while retaining scholarly integrity, transparently integrating
a rich distributed version control backend, could help reduce the friction of
contribution to a wide range of projects.

DH Abstract
===========

The Son of Suda On Line (SoSOL) is one of the main components of the
Integrating Digital Papyrology project (IDP), aiming to provide a repurposable
web-based editor for the digital resources in the DDbDP and HGV. SoSOL
integrates a number of technologies to provide a truly next-generation online
editing environment. Using JRuby with the Rails web framework, it is able to
take advantage of Rails’s wide support in the web development community, as
well as Java’s excellent XML libraries and support. This includes the use of
XSugar to define an alternate, lightweight syntax for EpiDoc XML markup,
called Leiden+. Because XSugar uses a single grammar to define both syntaxes
in a reversible and bidirectional manner, this is ideal for reducing side
effects of transforming text in our version-controlled system. SoSOL uses the
Git distributed version control system as its versioning backend, allowing it
to use the powerful branching and merging strategies it provides, and enabling
fully-auditable version control. SoSOL also provides for editorial control of
changes to the main data repository, enabling the democracy of allowing anyone
to change anything they choose while preserving the academic integrity of
canonical published data. This talk will provide a demonstration of these
features of SoSOL as implemented for IDP2, as well as a discussion of its
repurposable design for applicability to other projects and the ongoing
documentation work being done to increase usability and adoption in the wider
community.

Next-Generation Version Control
-------------------------------
Many online editing environments, such as MediaWiki, use an SQL database as
the sole mechanism for storing revisions. This can lead to a number of
problems, such as scaling (most SQL servers are not performance optimized for
large text fields) and distribution of data (see for example the database
downloads of the English Wikipedia, which have been notoriously problematic
for obtaining the full revision history). Most importantly, they typically
impose a centralized, linear, single-branch version history. Because Git is a
distributed version control system, it does not impose any centralized
workflow. As a result, branching and merging have been given high priority in
its development, allowing for much more concurrent editing activity while
minimizing the difficulty of merging changes. SoSOL’s use of Git is to have
one “canonical” Git repository for public, approved data and to which commits
are restricted. Users and boards each get their own Git repositories which act
as forks of the canonical repository. This allows them to freely make changes
to their repository while preserving the version history as needed when these
changes are merged back into the canonical repository. These repositories can
also be easily mirrored, downloaded, and worked with offline and outside of
SoSOL due to the distributed nature of Git. This enables a true democracy of
data, wherein institutions still retain control and approval of the data which
they put their names on, but any individual may easily obtain the full dataset
and revision history to edit, contribute to, and republish under the terms of
license.

Alternative Syntax for XML Editing
----------------------------------
While XML encoding has many advantages, users inexperienced with its use may
find its syntax difficult or verbose. It is still desirable to harness the
expertise of these users in other areas and ease their ability to add content
to the system, while retaining the semantically explicit nature of XML markup.
To do this, we have used XSugar to allow the definition of a “tagless” syntax
for EpiDoc XML, which resembles that of the traditional printed Leiden
conventions for epigraphic and papyrological texts where possible. Structures
which are semantically ambiguous or undefined in Leiden but available in
EpiDoc (e.g. markup of numbers and their corresponding value) have been given
additional text markup, referred to comprehensively as Leiden+. XSugar enables
the definition of this syntax in a single, bidirectional grammar file which
defines all components of both Leiden+ and EpiDoc XML as correspondences,
which can be statically checked for reversibility and validity. This provides
much more rigorous guarantees of these properties than alternatives such as
using separate XSLT stylesheets for each direction of the transform, as well
as encoding the relation between the components of each syntax in a single
location.

Repurposable Design
-------------------
Due to institutional requirements, the DDbDP and HGV datasets needed separate
editorial control and publishing mechanisms. In addition, their control over
different types of content necessitated different editing mechanisms for each
component. These requirements informed the design of how SoSOL interacts with
data under its control and how this design is repurposable for use in other
projects. The two high-level abstractions of data made by SoSOL are
“publications” and “identifiers”. Identifiers are unique strings which can be
mapped to a specific file path in the repository, while publications are
arbitrary aggregations of identifiers. By defining an identifier superclass
which defines common functionality for interacting with the data repository,
we can then subclass this to provide functionality specific to a given
category of data. The SoSOL implementation for IDP2, for example, provides
identifier subclasses for DDbDP transcriptions, HGV metadata, and HGV
translations. Editorial boards consequently have editorial control for only
certain subclasses of identifiers. Publications in turn allow representation
and aggregation of the complex many-to-many relationships these components can
have (for example, a document with two sides that may have one transcription
and two metadata components). Packaging these related elements together both
allows the user to switch between them and editorial boards to check related
data which they may not have editorial control over but still require to make
informed decisions about validity and approval. SoSOL can thus be integrated
into other systems by implementing the identifier subclasses necessary for the
given dataset as well as coherent means for aggregating these components into
publications.