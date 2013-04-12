Rationale - why Anthrax?
===================================

The form generation and parsing is one of the most common tasks in web (and not
only web) development. There is a number of challenges that must be faced by
any form package. The simplest designs either trade rich feature set for
portability or the other way around. Anthrax on the other hand tries to
maintain these two things.

    * The anthrax forms should be usable in different web and
      non-web environments).
    * The anthrax forms should be able to express all the
      features that can be used in some environemt, possibly falling back when
      some of the features aren't accessible)

The approach used to achieve this is high modularity. Anthrax is split into
multiple packages that are responsible for handling of some types of input
or rendering the widgets in different environments.
