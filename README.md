# SG16 Meeting Summaries

SG16 meetings are typically held on Wednesdays from 3:30pm-5:00pm EST5EDT4 on the 2nd
and 4th weeks of each month, but scheduling conflicts or other time pressures sometimes
force alternative scheduling.  Meeting invitations are sent to the mailing list and
prior attendees.

The next SG16 meeting is scheduled for Wednesday, December 11th 2019, from 3:30-5:00pm EDT.

- [November 20th, 2019](#november-20th-2019)
- [October 23rd, 2019](#october-23rd-2019)
- [October 9th, 2019](#october-9th-2019)
- [September 25th, 2019](#september-25th-2019)
- [September 4th, 2019](#september-4th-2019)
- [August 21st, 2019](#august-21st-2019)
- [July 31st, 2019](#july-31st-2019)
- [June 26th, 2019](#june-26th-2019)
- [June 12th, 2019](#june-12th-2019)
- [May 22nd, 2019](#may-22nd-2019)
- [May 15th, 2019](#may-15th-2019)
- [April 24th, 2019](#april-24th-2019)
- [April 10th, 2019](#april-10th-2019)
- [March 27th, 2019](#march-27th-2019)
- [March 13th, 2019](#march-13th-2019)
- [February 13th, 2019](#february-13th-2019)
- [January 23rd, 2019](#january-23rd-2019)
- [January 9th, 2019](#january-9th-2019)
- [December 19th, 2018](#december-19th-2018)
- [December 5th, 2018](#december-5th-2018)
- [October 17th, 2018](#october-17th-2018)
- [October 3rd, 2018](#october-3rd-2018)
- [August 29th, 2018](#august-29th-2018)
- [July 25th, 2018](#july-25th-2018)
- [July 11th, 2018](#july-11th-2018)
- [June 20th, 2018](#june-20th-2018)
- [May 30th, 2018](#may-30th-2018)
- [May 16th, 2018](#may-16th-2018)
- [April 25th, 2018](#april-25th-2018)
- [April 11th, 2018](#april-11th-2018)
- [March 28th, 2018](#march-28th-2018)
- [Prior std-text-wg meetings](#prior-std-text-wg-meetings)


# November 20th, 2019

## Draft agenda:
- Belfast follow up and review.
- Volunteers to draft a library design guidelines paper.

## Meeting summary:
- Attendees:
  - JeanHeyd Meneide
  - Mark Zeren
  - Steve Downey
  - Tom Honermann
  - Yehezkel Bernat
  - Zach Laine
- [P1868 - 🦄 width: clarifying units of width and precision in std::format](https://wg21.link/p1868):
  - Tom introduced the topic:
    - Concerns were raised in Belfast with regard to the stability of the proposed code point ranges to be
      used for display width estimation.  The currently proposed ranges map all extended grapheme clusters
      (EGCs) to a display width of one or two despite there being a number of known cases of EGCs that consume
      no display width (e.g., `U+200B {ZERO WIDTH SPACE}`) or more than two display width units
      (e.g., `U+FDFD {ARABIC LIGATURE BISMALLAH AR-RAHMAN AR-RAHEEM}`).
    - Additionally, the EGC breaking algorithm is dependent on Unicode version and the proposed wording does
      not specify which version of Unicode to implement.  Concerns were raised regarding having a floating
      reference to the Unicode standard and the potential for differences in behavior across implementations
      if the Unicode version is implementation defined and subject to change across compiler versions.
    - How should we address these concerns?
  - Zach commented that the wording review went through LWG ok and that he had posted a message to the LWG
    mailting list responding to one concern that was raised.
  - Zach reported that Jonathan Wakely stated that floating references to other standards are not permitted
    but that implementors can, as QoI, offer support for other versions.
  - Tom expressed surprise regarding that restriction given that we have a floating reference to ISO 10646
    in the working paper today.
  - Zach responded that LWG stated a requirement for a normative reference and is therefore planning to add
    a normative reference to Unicode 12 with the intent that we update the normative reference with each
    standard release.
  - Tom asked that, if we reference a particular version, can implementations use a later version and remain
    conforming.
  - Zach responded that doing so seems to be acceptable to implementors.
  - Steve remarked that CWG expressed a preference for a floating reference.
  - JeanHeyd confirmed and added that is how the working paper ended up with the floating reference to ISO 10646.
  - Zach said he will follow up about this discrepancy.
  - Mark asked if we have a preference for floating vs fixed.
  - Zach responded that implementations will do what they need to do for their users.
  - Tom turned the discussion back to concerns raised by Billy regarding changes to the width estimate algorithm
    being a breaking change; e.g., changing the width estimate for a given EGC.  This is a related but distinct
    concern from the EGC algorithm changing due to a change in Unicode version.
  - Zach stated that `U+FDFD` is an example of something we need to fix that can also be a breaking change.
  - Steve repeated that the concern is basically any change in behavior potentially resulting in a surprising or
    undesirable change.
  - Mark asserted that we're going to continue having difficulties with dependencies on Unicode data and that the
    situation is analagous with respect to the timezone database.  Implementors can enable stable behavior by
    allowing choice of Unicode version.
  - Steve noted that the rate of change of the Unicode standard has skewed towards stability.
  - Mark opined that we should not solve this problem in the standard.
  - Tom agreed and added that we can specify a minimum version, but leave the atual version implementation defined.
  - Mark asked which version of the Unicode standard the proposed code point ranges were pulled from.
  - Tom responded that the Unicode standard doesn't contain character display width data and that these were
    extracted from an implementation of `wcswidth()`.
  - Steve stated that he maintained a list of double wide characters for years and that it was not a significant
    burden.
  - Tom stated that his desire for a floating reference to the Unicode standard with an implementation defined
    choice of version is intended to allow implementors to keep up with new Unicode versions.  Unicode releases
    happen every year while C++ standards are only released every three years.  Implementors probably can't lag
    Unicode by three years.
  - Zach acknowledged the goal and stated that will result in some implementation divergence as some implementors
    will keep up and some won't, but that the differences are likely to be minor.
  - Tom asked if ISO 10646 annex U constitutes a reference to [UAX#31](http://www.unicode.org/reports/tr31).
  - Steve suggested this is probably a beuracratic issue and added that having a normative reference is helpful.
  - Zach responded that it could be harmful if we get cconflicting floating and non-floating references for
    ISO 10646 vs Unicode, but this should fall to LWG and CWG to decide.
  - Tom asked how we should go about fixing the currently proposed width estimates since the proposed ranges are
    clearly missing support for cases of zero width or width greater than two.
  - Zach opined that he wasn't sure there is a problem to be fixed since what is specified matches existing practice.
  - Tom asked if we know where this implementation of `wcswidth()` came from and how widely deployed it is.
  - Zach suggested asking Victor.
  - \[Editor's note: According to [P1868R0](https://wg21.link/p1868r0), the implementation of `wcswidth()` is the
    one at https://www.cl.cam.ac.uk/~mgk25/ucs/wcwidth.c. \]
  - Tom asked for opinions regarding writing a short paper that explains the Unicode stability guarantees and argues
    for floating references and implementations.
  - Zach suggested waiting for a more motivating reason to do so.
- [P1949 - C++ Identifier Syntax using Unicode Standard Annex 31](https://wg21.link/p1949):
  - Tom introduced the topic:
    - EWG rejected the SG16 guidance offered in response to NB comment
      [NL029](https://github.com/cplusplus/nbballot/issues/28) to deprecate identifiers that do not conform to
      [UAX#31](http://www.unicode.org/reports/tr31) with noted exceptions for the `_` character.
    - A suggestion was made that a CWG issue be filed to consider the lack of updates to the allowed identifiers
      since C++11 as a defect.
    - Tom agreed to file a core issue and started to do some research.
    - According to [N3146](https://wg21.link/n3146), the original identifier allowances appear to have been
      aggregated from various sources including
      [UAX#31](http://www.unicode.org/reports/tr31) and
      [XML 2008](http://www.w3.org/TR/2008/REC-xml-20081126/#sec-common-syn), and following guidance in
      annex A of a draft of
      [ISO/IEC TR 10176:2003](http://www.open-std.org/JTC1/sc22/WG20/docs/n970-tr10176-2002.pdf).
    - Thank you to Corentin for quickly providing a way to query the code point ranges that have the `XID_Start`
      or `XID_Continue` property set.  https://godbolt.org/z/h7ThEh.  These ranges differ substantially from
      what is in the current standard.
    - What should the proposed resolution for the core issue be?
  - Steve stated that [UAX#31](http://www.unicode.org/reports/tr31) permits extensions, and what was adopted for
    C++11 effectively whitelisted a large set of code points.
  - Zach asked what EWG's concern was.
  - Steve replied that they were nervous about such a late change and want more time to think it through.
  - Zach opined that this seems like something better addressed in C++23.
  - Steve noted that what is done can be back ported to prior standards though, that Clang and gcc support
    Unicode encoded source code \[Editor's note: so does MSVC \], and that the longer we wait to address this,
    the more code we potentially break.
  - Tom stated that, from the DR perspective, we could either figure out what we want for C++23 and recommend
    that as the proposed resolution, or we can do a more targetted fix for C++20 for specific problematic
    cases knowing that we'll likely do differently for C++23.
  - Steve stated that the only difference C++ needs from [UAX#31](http://www.unicode.org/reports/tr31) is support
    for `_`, and such an extension is conforming.  It would also be ok to restrict identifiers to a common script
    to avoid homoglyph attacks.
  - Steve added that there is also the issue of normalization forms and that gcc will currently warn if
    identifiers are not in NFC form.
  - Mark asked if we should make it ill-formed for identifiers to not be in NFC form.
  - Steve responded that doing so could break existing code.
  - Tom suggested normalizing when comparing identifiers is another approach.
  - Steve noted that doing so requires the Unicode normalization algorithms.
  - JeanHeyd mentioned that we'll also have the problem of reflecting identifiers in the future and that
    normalization will be relevant there.  Corentin brought this up in SG7.  Requiring NFC would be helpful there.
  - Mark expressed support for the idea of requiring NFC.
  - Steve suggested that there is always the `universal-character-name` escape hatch.
  - Mark opined that EWG probably won't like requiring conversion to NFC in name lookup.
  - Tom responded that gcc is at least detecting non-normalized identifiers today, that doing so must require
    some level of Unicode database support, and that performance costs are presumably reasonable.
  - Steve stated that gcc looks for some range of combining code points and may not be 100% accurate.
  - Mark asked if non-NFC normalization can be detected without having to fully normalize?
  - Zach responded that he didn't think so.
  - Mark asked if normalization was brought up in EWG.
  - Steve responded that it wasn't, that we didn't get that far in the discussion.
  - Tom suggested that we have a good amount to think about here and that he is looking forward to the next
    revision of Steve's paper.
  - Steve took the bait and agreed that the paper will have to provide good arguments for why this is important.
  - Zach suggested that this should be easy for implementors if they don't have to deal with normalization and
    that we should just require NFC for performance reasons.
  - Mark asked if we could make use of non-NFC ill-formed NDR so that implementations are not required to diagnose
    violations.
- [P1097 - Named character escapes](https://wg21.link/p1097):
  - Tom introduced the topic:
    - EWG narrowly rejected the paper, but expressed good support for the direction.
    - Most concerns had to do with implementation impact and, in particular, the potential increase in compiler
      binaries.  Some distributed build systems distribute compilers as part of the build process and the
      additional latency imposed by incresing the size of compiler binaries adds cost.  Numbers haven't been
      obtained, but guesses were around 2MB, but could probably be reduced to under 600K.
    - One prominent EWG member was strongly opposed to the design because he would prefer a solution that avoids
      baking Unicode into the core language.  Something like a string interpolation solution that could call
      out to `constexpr` library functions to do character name lookup.
    - Martinho was working on an implementation in Clang at Kona, but Tom doesn't know the state of it or where
      to find it.  Tom reached out to Martinho via email, but didn't hear back.
    - Anyone have time and interest to experiment and produce some estimates to address the implementation
      impact concerns?
  - Steve stated that he could probably do some work on it and that the name DB should compress really well with
    use of a trie.
  - JeanHeyd suggested that the [UAX44-LM2](https://www.unicode.org/reports/tr44/#UAX44-LM2) compression scheme
    could help to reduce size.
  - Tom expressed uncertaintly that it would help much over a trie, but we could experiment and put the results
    in a paper.
  - Zach suggested splitting names that contain "with" in them since the suffixes that tend to follow "with" are
    highly repeated.
  - Tom noted that the algorithmically generated names could be specially handled as well.
  - Steve added that a tokenization approach could help too.
  - Tom asked if anyone might know of a link to Martinho's implementation.
  - Zach replied that a link was provided at some point, possibly in Slack.
  - \[Editor's note: Tom searched Slack, but failed to find a reference. \]
- [P1880 - uNstring Arguments Shall Be UTF-N Encoded](https://wg21.link/p1880):
  - Tom introduced the topic:
    - LEWG rejected the SG16 guidance offered in response to NB comment
      [FR164](https://github.com/cplusplus/nbballot/issues/162) to adopt P1880 for C++20.
    - What should we do next?
  - Zach expressed frustration that he was available when the NB comment and paper were discussed in LEWG, but
    that no one notified him that the discussion was happening.
  - Zach stated that, after the SG16 meeting, he went through all references to `std::basic_string` and added
    missing references to PMR strings and `std::basic_string_view`.  This research also identified a number of
    references that are deserving of more scrutiny.
  - Zach opined that this isn't very important for C++20 and that he will work on a revision for C++23, though
    not for the Prague meeting.
  - Zach stated he was surprised at how many references to these types he found in function templates.
- Tom asked for volunteers to draft a library design guidelines paper.
  - Tom introduced the topic:
    - During the
      [SG16 meeting on July 31st](https://github.com/sg16-unicode/sg16-meetings/blob/master/README.md#july-31st-2019),
      we discussed guidelines for when to add function overloads for each of `char`, `wchar_t`, `char8_t`,
      `char16_t`, and `char32_t` and he would like to have a library guideline paper that records our guidance.
    - Would anyone be interested and willing to work on this?
  - Zach expressed interest in doing so.
- Mark brought up a wording update email Zach sent to LWG with regard to [P1868](https://wg21.link/p1868):
  - Mark noted that the wording introduces a new term of art: "estimated display width units".
  - Zach responded that the new term was intentional; we're leaving the width estimation effectively unspecified
    for non-Unicode encodings.  Implementors expressed a preference for not having to document their choices and
    we didn't want to force embedded compilers to have to be Unicode aware.  So, we needed a non-Unicode term.
  - Tom noted that the wording appears to require embedded compilers to use the proposed Unicode algorithm if
    their execution character set is Unicode.
  - Zach acknowledged that would be the case.
  - Mark siggested that is probably what we want if they are actually doing Unicode.
  - Tom agreed and suggested such implementors could otherwise state that their execution character set is ASCII.
- Tom communicated that the next meeting will be on December 11th.


# October 23rd, 2019

## Draft agenda:
- P1844R0: Enhancement of regex
  - https://wg21.link/P1844R0
- P1892R0 - Extended locale-specific presentation specifiers for std::format
  - https://wg21.link/P1892R0
- P1859R0 - Standard terminology for execution character set encodings
  - https://wg21.link/P1859R0
  
## Meeting summary:
- Attendees:
  - David Wendt
  - Mark Zeren
  - Peter Brett
  - Steve Downey
  - Tom Honermann
  - Yehezkel Bernat
  - Zach Laine
- Tom initiated a round of introductions for new attendees.
- P1844R0: Enhancement of regex
  - https://wg21.link/P1844R0
  - Tom introduced the paper on behalf of the author:
    - The proposal is an expansion of `std::basic_regex` specializations.
    - We've discussed issues with `std::basic_regex` before.  The author has put significant effort into this
      proposal.  It includes wording.  We owe it to the author to set aside any biases and consider the benefits of
      this paper.
    - An implementation is available though it only implements the proposed `char8_t`, `char16_t`, and `char32_t`
      specializations, not the existing `char` or `wchar_t` specializations.
    - The paper does not propose an alternative to `std::basic_regex`, but rather attempts to address shortcomings
      of it for UTF encodings via specializations.  \[Editor's note: this implies that the proposal doesn't address
      issues with support of UTF encodings with the `char` and `wchar_t` specializations.\]
    - The paper proposes a new regex syntax option, `ECMAScript2019`, to be used to select a regular expression
      engine that implements the ECMAScript 2019 specification.  This option would be available for use with all
      `std::basic_regex` specializations.
    - The paper proposes a new `dotall` syntax option that allows the `.` character to match any Unicode code point,
      including new line characters, when using the `ECMAScript2019` option.
    - The new `ECMAScript2019` syntax option would be the only syntax option supported for the `char8_t`, `char16_t`,
      and `char32_t` specializations.
    - The `ECMAScript2019` regular expression engine would *NOT* exactly match the ECMAScript 2019 specification:
      - The `\xHH` expression is redefined to match code points rather than code units.  However,
      - The author would be fine with removing support for the `\xHH` expression since support for code points is
        provided by the `\uHHHH` and `\u{H...}` expressions.
    - The proposal removes locale dependency for the `char8_t`, `char16_t`, and `char32_t` specializations and
      therefore does not propose any new specializations of `std::regex_traits`.
    - The paper proposes new overloads of `std::regex_match` and `std::regex_search` to allow specifying look
      behind limits on ranges.
    - The proposed changes to `std::regex_iterator` are ABI breaking.
  - PeterBr observed that the proposal doesn't deal with language specific aspects like case folding.
  - PeterBr stated he liked the motivation for this paper and the notion that `std::regex` can be made to work.
  - Zach asked about support for collation and whether anyone was familiar with the existing `collate` syntax option.
  - PeterBr responded that the paper states that the `collate` option is ignored for these specializations.
  - Zach stated that the default collation is not useful and that tailoring is required.
  - Tom summarized, so the paper needs to address collation.
  - Zach refuted that need since it could profoundly impact performance.
  - PeterBr suggested that, perhaps, regex for Unicode should operate on `std::text`.
  - Tom expanded that suggestion to any sequence of code points and observed that the proposal kind of does that
    already via the changes to `regex_iterator`.
  - Zach agreed it would be useful to use as an adapter for code points.
  - Tom asked if a new regex feature for non-compile-time regex support would be preferred over specializing
    `std::basic_regex` as proposed.
  - Zach responded that he doesn't think `std::regex` is DOA, but if we're going to support Unicode regex with
    dynamic patterns, then, we should pursue some of the design of CTRE.
  - Zach added that solving the problem is important and that he wants to see Unicode regex support but would
    prefer to take a wait-and-see approach on this paper while watching how CTRE and `std::format` evolve.
  - PeterBr acknowledged the benefits of CTRE, but stated that we do need a solution for dynamic regex.
  - Zach reported that be believes that Hana is planning to make CTRE capable of supporting dynamic pattern
    strings and If that were to happen, then we wouldn't need `std::regex` any longer.
  - Mark lamented the lack of a proposal like this one when C++11 was being designed since the approach looks
    good relative to other papers from the past.
  - Mark added that it is an embarassment that we don't have a solution for this today, but that he feels kind
    of neutral on it as well due to concerns about allocating time for this relative to other things we could
    do.
  - Mark asked what implementors would think and if they get requests for Unicode `std::regex` support.
  - Mark asserted that the implicit use of the `ECMAScript2019` engine when a different syntax option is
    specified has to be changed.
  - Zach reiterated that this proposal is definitely an ABI break, that an ABI break is a serious problem, and
    that the need for such a break suggests we need a different family of types.
  - Mark added that the paper should make it clear that it does break ABI, not that it might.
  - Tom asked if this proposal solves the `std::basic_regex` issues with support for variable length encodings.
  - Zach responded that `std::regex` doesn't handle incomplete or ill-formed code unit sequences and suggested
    that perhaps those should match against `\uFFFD`.
  - Zach reported that `std::regex` can also match code unit ranges that stride code unit sequences since
    `std::regex` effectively matches bytes.
  - Tom asked what guidance we should offer to LEWG.
  - Zach suggested:
    - We should solve this problem.
    - This approach is premature given other things in flight now, but if this had been proposed three years
      ago he might have felt differently about it.
  - PeterBr suggested it should be prioritized behind CTRE.
  - Tom asked whether support for tailoring is important.
  - Zach suggested placing tailoring at the lowest priority and mentioned that he doesn't think ICU supports it
    as people don't often want to do collation aware searching.
  - Tom reiterated that we should offer guidance that it be ill-formed to specify a syntax option other than
    `ECMAScript2019` for the proposed specializations.
- P1892R0 - Extended locale-specific presentation specifiers for std::format
  - PeterBr introduced the paper:
    - Looking through the `std::format` specification he found that there are useful floating point formats that
      can not be produced in locale specific formats.
    - Locale specific formats are important in scientific fields.
    - The `'n'` specifier has a different meaning for integers than it does for floating point.
    - An NB comment was filed to make the `'n'` specifier indicate a locale specific format rather than a type
      modifier.
    - The proposed change should not affect existing well-formed `std::format` calls except for `bool` which
      would now be formatted as locale variants of "true" or "false" instead of 1 or 0.
    - This would make `std::format` unambiguously the best choice for localized formatting since locales can
      be easily specified and `std::format` already solves short falls of iostreams and printf such as ordering.
    - Without this change, there is still a need to use `printf` for locale sensitive formatting.
  - Mark noted that this change will break existing users of [{fmt}](https://github.com/fmtlib/fmt).
  - PeterBr responded that it will for existing uses of `bool` but that he isn't concerned about existing users
    of [{fmt}](https://github.com/fmtlib/fmt).
  - Tom observed that use of `'l'` as the specifier as suggested in the paper avoids the break and aligns with
    Victor's [P1868R0](http://wg21.link/p1868r0) paper to enable locale specific handling of character encodings.
  - Mark stated that the core issue is that there remains some uses of `printf` that can't be directly replicated
    with `std::format` and asked how a programmer would print, for example, the locale specific decimal character
    but without the locale specific thousands separator.
  - PeterBr responded that the programmer can create a custom locale.
  - Zach stated that we can't defer this until C++23 because changing the meaning of `'n'` would break compatibility
    and asked why we can't just introduce an `'l'` specifier in C++23?
  - PeterBr responded that doing so makes things more complicated and asked whether we would deprecate `'n'` if
    `'l'` were to be adopted.  We can postpone addressing this, but we get a cleaner solution in the long term by
    addressing it now.
  - Zach agreed with the motivation being to avoid a wart that we'll need to teach but that some opposition will be
    raised due to perceived risk at this late stage.
  - Zach stated that he likes the change, but that it needs good motivation.
  - PeterBr suggested that `'n'` could be removed now and then restored with desired changes in C++23.
  - Zach suggested that if Victor supports the paper, it will probably pass, but if he disagrees with it, then it is
    probably DOA.
  - Mark stated that the choices need to be clearly presented for LEWG.
  - Zach observed that there are a few options and suggested presenting a cost/benefit of each so that LEWG is given
    clear choices.
  - Mark suggested socializing the issue on the LEWG mailing list now to flush out any objections.
  - PeterBr stated that any help improving the paper would be appreciated.
  - Mark suggested presenting either slides or a different paper that presents the options and analysis.
  - PeterBr stated he would create a doc that could be collaboratively edited.
- P1859R0 - Standard terminology for execution character set encodings
  - Steve introduced the paper:
    - The goal is to not affect implementations, but rather to fix wording so that we can use modern terminology
      and understand each other better.
    - We often use terms like "execution encoding" that are not defined in the standard and are opportunities for
      confusion.
    - We need to admit that `wchar_t` is not, in practice, able to hold all code points of the wide execution
      character set.
  - Zach asked what "literal encoding" is for.
  - Steve responded that it reflects the encoding for non-UTF literals.
  - Zach asked what difference is intended by "character set" and "character repertoire".
  - Steve responded that the goal is to tighten up the meanings of existing terminology so as to avoid massive
    changes to the standard.
  - Mark observed that there seems to be a missing word in the definition of "Basic execution character set"; that
    there seems to be a missing "that".
  - PeterBr stated that this should be high priority in C++23 so we can get everyone on board with terminology.
  - Steve agreed and asserted we'll need to socialize these new terms.
  - Tom asked if there are any terms being dropped; it looks like the paper adds "literal encoding" and
    "dynamic encoding".
  - Steve responded that none are dropped and stated there will be an additional associated encoding added for
    character types as well.
  - Mark noticed that the paper discusses `literal_encoding` and `wide_literal_encoding` but doesn't define a term
    for "Wide literal encoding".
  - Tom asked if "source encoding" should be added.
  - Tom asked if we should add a statement that the dynamic encoding must be able to represent all of the characters
    of the execution character set.
  - Steve responded that we could add that.
  - PeterBr observed a potential problem with doing so on Windows where the dynamic encoding might be UCS-2, but the
    execution character set is UTF-16.
  - Tom suggested refining the requirement such that characters used in literals must have a representation in the
    dynamic encoding.
  - Mark suggested it would be helpful to have a cheat sheet with mathematical notation of which terms denote a
    subset of other terms.
  - Steve agreed.
  - Tom suggested that we also need "wide dynamic encoding".
  - Zach asked about the difference between the "encoding" and "character set" terms.
  - Steve responded that the former states how characters are represented while the latter states what characters
    must be representeable.
  - Zach stated it would be useful to have text explaining the difference.
  - Tom asked how ODR violations would be avoided for `literal_encoding` since literal encoding can vary by TU.
  - Steve responded that the same technique used for `std::source_location` can be used; a value is provided.
- Tom confirmed that the next meeting will be November 20th.


# October 9th, 2019

## Draft agenda:
- P1880R0 - u8string, u16string, and u32string Don't Guarantee UTF Endcoding
  - https://github.com/tzlaine/small_wg1_papers/blob/master/P1880_uNstring_shall_be_utf_n_encoded.md
- P1879R0 - The u8 string literal prefix does not do what you think it does
  - https://github.com/tzlaine/small_wg1_papers/blob/master/P1879_please_dont_rewrite_my_string_literals.md
- P1844R0: Enhancement of regex
  - https://wg21.link/p1844

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - David Wendt
  - Henri Sivonen
  - JeanHeyd Meneide
  - Peter Bindels
  - Peter Brett
  - Tom Honermann
  - Zach Laine
- P1880R0 - u8string, u16string, and u32string Don't Guarantee UTF Endcoding
  - https://github.com/tzlaine/small_wg1_papers/blob/master/P1880_uNstring_shall_be_utf_n_encoded.md
  - Zach introduced:
    - The idea is that interfaces taking these string types expect that contents of these strings are
      well-formed UTF-8, UTF-16, UTF-32 respectively; this requirement needs to be reflected in the standard.
    - We should state a blanket requirement for these expectations.
    - The paper proposes a 4th bullet to [[res.on.arguments]](http://eel.is/c++draft/res.on.arguments).
  - PeterBr asked if the requirement should be for well-formed data.
  - Zach replied that it should be.  LWG should confirm that.
  - Henri asked what happens if an ill-formed code unit sequence is passed.  Is it undefined behavior or as-if
    the Unicode replacement character was present?
  - Zach replied that the current wording makes it undefined behavior.
  - PeterBr provided an example of why the behavior is undefined.  Consider a string that ends with an incomplete
    code unit sequence; the implementation could run off the end of the buffer.
  - Zach responded that, for `std::basic_string` types, the buffer overrun can be avoided, but in that case, the
    interface specification should state that behavior.  The proposed blanket wording is for the weakest interface
    requirements and can be strengthened by individual interfaces.
  - Henri asked if that is useful as it seems like undefined behavior is a huge foot cannon; replacement character
    semantics would provide a safer interface.
  - Zach responded that, if this is a foot gun, then so is `std::vector operator[]`.  You must meet preconditions.
    Implementations can always constrain their handling if they want.  The intent here is to enable the fast path.
  - PeterBr added that it would add complexity to implement replacement character behavior; interfaces would not
    be able to use SIMD instructions if ill-formed strings must be handled.
  - Zach repeated that the proposal just specifies the default behavior unless otherwise specified for an interface.
  - Corentin opined that this seems almost editorial.
  - Henri stated that, for `char8_t`, there are values that are never valid in well-formed UTF-8 text and asked what
    an individual `char8_t` means; it must be restricted to ASCII.
  - Tom noted that matches UTF-8 character literals; they can only specify ASCII values.
  - Zach read the existing content in [[res.on.arguments]](http://eel.is/c++draft/res.on.arguments) in order to
    demonstrate similarity in existing requirements.
  - Henri asked if this represents a requirement that is more difficult to satisfy than the existing requirements.
    For example, in UTF-16, almost all code bases will allow unpaired surrogates.  Does this requirement make the
    standard library useless for their code bases?
  - Zach stated that interfaces can specify their handling of unpaired surrogates.
  - Henri asked again if this is a practical requirement.
  - Tom responded that this is needed for our mantra of not leaving performance on the floor; we can't both check
    for ill-formed text and maximize performance.
  - Zach added that ICU already does this for performance.  Within Boost.Text, Zach added interfaces for both
    unchecked and checked text.
  - PeterBr opined that this paper is great and sorely needed.
  - Tom and Corentin agreed.
  - Henri asked Zach to expound on his statement that ICU already exhibits undefined behavior.
  - Zach responded that, in ICU normalization code, assumptions are made when decoding UTF-8.  For example, unsafe
    unpacking of UTF-8 is performed.
  - Henri asked if ICU does likewise for UTF-16 for unpaired surrogates.
  - Zach responded that he thought so, but is not completely sure.
  - Corentin expressed support for an NB comment to include this in C++20.
  - Tom opined that it doesn't much matter if this makes C++20 as implementors will already do the right thing.
  - Henri asked if this might introduce a backward compatibility issue in C++23 if added after C++20.
  - Tom responded that the undefined behavior is effectively already there; this is fixing an underspecification.
  - Henri stated it would be a huge task to scrub existing code bases to avoid this undefined behavior.
  - Zach predicted that we'll end up with separate interfaces for assuming an encoding vs checking the encoding.
    This isn't hurting anybody, it is just enabling fast path implementations.
  - Henri expressed concern about digging deeper into making default interfaces unsafe; like `std::optional::operator*`
    is.  He would prefer unsafe interfaces be clearly marked as unsafe.  This undefined behavior has the potential
    to introduce security issues.
  - Zach responded that most standard interfaces are unsafe in some way, for example every function that accepts
    arguments of pointer type.
  - Henri countered that the undefined behavior can be avoided in this case; just like we could for
    `std::optional::operator*`.
  - Zach suggested that C++ is often used for its performance advantages; we want the default to be fast.  But this
    proposal isn't really about that; it is about documenting our default behavior.
  - PeterBr stated that `std::u8string` is `std::basic_string` with `char8_t`.  `std::basic_string` provides many
    interfaces that allow mutating the string in a way that would break otherwise well-formed UTF-8.  Rust doesn't
    do that.  We could specify a UTF-8 string type that maintains invariants, but it wouldn't be a `std::basic_string`
    any more.  Thus, it is up to the programmer to not violate UTF-8 requirements.
  - Corentin agreed that we don't want to change `std::u8string`; it is just a container of code units.  String
    mutation should be managed via some overlying type like `std::text`.  This paper just reflects existing behavior.
  - Henri asked if we really want to enable so much performance that we risk our users.  In Firefox, lots of string
    checking is done to avoid security issues even though ill-formed UTF-8 is very rare.  The performance isn't bad.
  - PeterBr responded that an implementation can choose to define its behavior.
  - Henri countered that, if it isn't required everywhere, then it can't be relied on.
  - Corentin suggested that, if you want safety, then `std::basic_string` is not the type you're looking for.  We're
    going to need other types on top and, eventually, we'll have more trusted types.
  - Zach added that no interfaces are being specified in this paper, so there are no ergonomic concerns.  Again, this
    is just proposing blanket wording that can be strengthened in individal interfaces.
- Tom initiated a discussion about polling during telecons.
  - Tom introduced:
    - He prefers to avoid polling during telecons in favor of polling during face to face meetings.  This is due
      to 1) larger numbers of attendees at face to face meetings, 2) more opportunity for input from those that do
      not regularly attend telecons, and 3) more opportunity for background thinking after a discussion before having
      to respond to a poll.
    - He also sees the telecons as useful for priming discussion and identifying non-obvious concerns.
  - Tom asked if anyone wanted to argue for a change in practice.
  - The group expressed general agreement to continue doing what we've been doing.
- P1879R0 - The u8 string literal prefix does not do what you think it does
  - https://github.com/tzlaine/small_wg1_papers/blob/master/P1879_please_dont_rewrite_my_string_literals.md
  - Zach introduced:
    - This started from an experience from a while back that we have previously discussed.
    - Tests involving UTF-8 formatted source files failed when compiled with the Microsoft compiler, but not with
      other compilers.
    - The source files did not have a UTF-8 BOM and Microsoft's `/source-charset:utf-8` option wasn't being used,
      so the source files were decoded as Windows-1252.
    - String literals therefore did not contain what was expected because code units were not interpreted as expected.
    - The paper proposes prohibiting use of `u8`, `u`, and `U` literals unless the source file encoding is a Unicode
      encoding.
  - Corentin suggested relaxing the prohibition to allow use of these literals so long as the source contents of the
    literal only use characters from the basic source character set.  \[Editor's note: presumably this would still
    allow characters outside the basic source character set if specified with `universal-character-name` escape
    sequences.\]
  - Corentin also stated that the current behavior makes sense according to the standard, but most programmers aren't
    aware of source file encoding vs execution encoding concerns.
  - Henri stated that the behavior makes sense if you think of C++ source code as text rather than bytes and agreed
    that this isn't what programmers expect.
  - PeterBr expressed support for the paper because it ensures you get the same abstract characters written in the
    source file and added that it would be nice if this paper used the same terminology as propsoed in Steve's recent
    terminology paper ([P1859R0](https://wg21.link/p1859r0)).  \[Editor's note: this paper will be in the Belfast
    pre-meeting mailing.\]
  - Zach agreed regarding use of terminology.
  - Tom expressed concerns regarding breaking backward compatibility, particularly for z/OS where source files are
    EBCDIC and `u8` literals are used to produce ASCII strings.
  - Zach asked if it would help to only allow characters from ASCII.
  - PeterBr stated that, if the compiler is not explicitly told what the source encoding is, you are in trouble since
    the compiler can't always detect an encoding expectation mismatch.
  - Henri noted that the translation model matches what is done on the web where HTML source is transcoded to some
    internal (Unicode) encoding.  A compiler could preserve meta data about the encoding a literal came from and,
    if the transcoded code point is above 0x80, issue a diagnostic.
  - Zach asked for more information regarding concerns for z/OS and EBCDIC.
  - Tom explained the source translation model according to [translation phase 1](http://eel.is/c++draft/lex.phases#1.1).
    Source files are first transcoded from an implementation defined encoding to an implementation defined internal
    encoding.  The internal encoding has to be effectively Unicode (or isomorphic to it) due to possible use of
    `universal-character-name` sequences in the source code.  The internal encoding is then transcoded to the various
    execution encodings where needed.
  - Tom went on to explain that there are multiple EBCDIC code pages and that many of the characters available in
    them are not defined in ASCII.  Restricting UTF literals to just ASCII would prevent use of those characters.
  - Tom restated PeterBr's point from earlier.  This problem is always due to mojibake; the source file being
    encoded in something other than what the compiler expects.
  - PeterBr agreed that the root cause is the encoding mismatch and opined that this is a problem worth solving.
    The question is how best to solve it.  The first place to look is at the translation from source encoding to
    internal encoding.
  - Henri expressed belief that it makes sense to address the problem where Zach suggests.
  - Zach stated that the right place to detect this is during parsing; when parsing a UTF literal, it is critical
    to know what the source encoding is.
  - Tom countered that it is necessary to know the encoding as soon as you hit a code unit that doesn't represent a
    member of the basic source character set.
  - Henri stated that diagnosing any such code unit is a harder sell than just diagnosing one in a UTF literal.
  - Tom agreed.
  - PeterBr noted that it is implementation defined how (or if) characters outside the basic source character set
    are represented.  The goal of the paper is effectively to tighten that up.  That means implementations can
    have extensions to relax diagnostics.
  - Henri responded that such arguments apply to any change to the standard.
  - Zach agreed, but noted this is restricted to source files that have UTF literals with transcoded code points
    outside of ASCII.
  - Henri stated that there is more potential for failures for some character sets than others.  For example,
    some character sets don't roundtrip through Unicode.  This failure mode already exists, but there is little value
    in trying to diagnose this outside of UTF literals.
  - PeterBr stated that a source file with code units representing characters outside of the basic source character
    set is ill-formed subject to implementation defined behavior.  When a programmer writes a UTF literal, that is a
    request for a specific encoding, but it is perfectly valid for the source file to be written in Shift-JIS.
  - Henri acknowledged that perspective as logically valid, but doesn't address the problems caused by the Microsoft
    compiler's default behavior not matching user expectations.  Programmers are using UTF-8 editors these days.
  - PeterBr asserted that is a quality of implementation concern and not an issue with the standard.
  - Tom agreed.
  - Zach stated that the proposed restrictions can be worked around by using `universal-character-name` escapes
    and stated a preference for implementing a solution that results in a diagnosis for the problem he encountered,
    but that this isn't a critical issue.
  - Corentin brought up static reflection and that, at some point, reflection will require defining or reflecting
    the source file encoding.
  - Tom stated that dovetails nicely with Steve's P1859R0 draft that provides a callable for conversion of string
    literal encoding.
  - Corentin noted that Vcpkg compiles all of its packages with the Microsoft compiler's `/utf-8` option and that
    Microsoft may be open to defaulting source encoding to UTF-8 when compiling as C++20.
  - Zach added that the Visual Studio editor, by default, adds a UTF-8 BOM to new source files it creates, though
    it doesn't implicitly add a UTF-8 BOM when existing files are added to a project.
  - Corentin observed that, because source encoding is not portable, most programmers just don't use characters
    outside of ASCII except in comments; which is why such characters are ignored.
  - PeterBr suggested that an evening session in Belfast to discuss this or other ideas might be an option and that
    it would be good to talk directly with implementors.
- Tom confirmed that the next meeting will be on October 23rd and will be the last meeting before Belfast.


# September 25th, 2019

## Draft agenda:
- Discuss LWG#3290 - Are std::format field widths code units, code points, or something else?
  - https://cplusplus.github.io/LWG/issue3290
  - Victor plans to have a draft paper for discussion.
- Discuss P1844R0: Enhancement of regex
  - https://wg21.link/p1844


## Meeting summary:
- Attendees:
  - Corentin Jabot
  - JeanHeyd Meneide
  - Lyberta
  - Mark Zeren
  - Tom Honermann
  - Victor Zverovich
  - Zach Laine
- Discuss D1868R0 - 🦄 width: clarifying units of width and precision in std::format
  - http://wiki.edg.com/pub/Wg21belfast/SG16/D1868R0.html
  - Addresses https://cplusplus.github.io/LWG/issue3290
  - Victor introduces:
    - Any solution to this problem must deal with conflicting constraints.  The programmer's intention is
      to align text output assuming a monospace font and some understanding of how the text will be rendered
      (e.g., how many terminal columns will be consumed by each "character").  Implementors desire a clear
      and precise specification; preferably one that does not have great complexity that may lead to
      reliability issues or bug reports.
    - Field precision is more consequential than field width because it truncates text potentially resulting
      in ill-formed output if truncation doesn't occur at a suitable boundary.
    - Experimentation with an approach that estimates field width based on Unicode's extended grapheme clusters
      and script blocks produced good results; better results than estimation based on code point counts.
    - Experimentation on macOS, Linux, and Windows revealed that Windows currently has the most significant
      limitations with regard to support for Unicode characters currently not represented in Microsoft's
      supported ANSI code pages.  Experiments have not been performed using the new Windows terminal which
      may be expected to produce better results.
    - Testing of Unicode family emoji demonstrated the most variability of results since family emoji may be
      rendered as a single glyph or as a series of glyphs each representing a family member.
    - Field width is an estimate.  Unless apriori knowledge of how the text will actually be rendered is
      available, the width of any given text can only be approximated.
    - The experimental implementation uses [Boost.text](https://github.com/tzlaine/text) to identify extended
      grapheme cluster boundaries and computed width based on Unicode block ranges culled from an
      implementation of `wcswidth`.
  - Corentin mentioned that the issue with family emoji extends to other sequences of combining emoji.  For
    example, ninja cat (`U+1F431 {CAT FACE}`, `U+200D {ZERO WIDTH JOINER}`, `U+1F464 {BUST IN SILHOUETTE}`) is
    rendered with a single glyph on Windows, but currently with two glyphs on Linux.  Width fundamentally
    depends on rendering.
  - Corentin added that, for non-Unicode encodings, width estimation must look at code units and do things
    differently for double-byte characters vs single-byte characters.
  - Victor stated that he is content with handling of non-Unicode encodings being implementation defined.
  - Zach agreed and asserted that we want a 90% solution.  Support of non-Unicode encodings would require
    information that we can't currently specify in the `std::format` interface assuming `std::format`
    remains locale independent; it is ok for implementations to assume an encoding.
  - Tom thanked Victor for doing this research and stated he found it sufficiently compelling to take the code
    unit solution he previously advocated for resolving [LWG 3290](https://wg21.link/lwg3290) off the table.  In
    particular, the demonstration of prior art in the form of POSIX
    [`wcswidth`](https://pubs.opengroup.org/onlinepubs/9699919799/functions/wcswidth.html) lent confidence to
    this approach.
  - Tom asked if width calculation for `wchar_t` could be delegated to `wcswidth`.
  - Victor replied that `wcswidth` is locale dependent and that goes against the `std::format` design.
  - Tom asked if width calculation for `char` and `wchar_t` couldn't be implementation defined such that an
    implementation could query locale only when width or precision is explicitly specified and the arguments
    are characters or strings.  Width or precision specifiers would effectively constitute an opt-in for locale
    dependence.
  - Zach objected on the basis that dependence on locale could cause output to differ on one platform vs another
    for the same character or string data.
  - Victor clarified that, if encoding doesn't match, the worst case result is mis-alignment.
  - Corentin stated that, as currently specified, `std::format` formats bytes since it doesn't know the precise
    encoding of inputs.  Correct text manipulation requires knowing the encoding.
  - Corentin expressed agreement that display width is what programmer's expect.  Perhaps in C++23, the ability
    to pass an encoding argument could be added.
  - Tom mentioned that `std::format` can take a `std::locale` option from which the encoding could be queried
    thus making it possible for programmers to opt-in to locale awareness simply by passing a locale object.
  - Zach again objected based on the desire to have portable output.
  - Corentin expressed a strong preference for a good solution in C++20 and asked if we could specify that width
    and precision units are display width and, for characters outside the basic source character set, behavior is
    implementation defined.
  - Victor stated that is a minimum viable solution.  The paper proposes that encoding is an implementation
    defined fixed encoding, not a run-time selected one.
  - Corentin confirmed satisfaction with a minimal solution for C++20 that we can iterate on for C++23 and that
    retains some flexibilty.
  - Zach observed that, if we make it implementation defined today, then we'll be stuck with implementation
    choices.  If the standard doesn't specify behavior, then implementors will choose one and we'll get stuck
    either way.  This is similar to breaking ABI; it can be an over-my-dead-body issue.
  - Corentin again expressed a desire for some way to preserve the ability to make changes later.
  - Zach stated that it is important to remember what Victor said previously; width is an estimate.
  - Mark observed that what we're discussing is mostly an edge case since most fields are aligned for numeric output.
  - Tom countered that alignment is useful for things like names.
  - Tom asked if `std::format` is constexpr.
  - Victor replied that parsing of the format string is constexpr, but actual formatting is not.
  - Corentin stated it would be useful to have constexpr formatting at some point, but querying locale would prevent
    that.
  - Tom disagreed and stated that an implementation could use an internal locale if formatting at compile-time.
  - Tom summarized his perceptions of our positions so far:
    - We appear to have agreement for display widths in some form.
    - We have disagreements over adding a locale dependency as part of encoding assumption.
  - Corentin asked Zach if he thought a best attempt at display width is sufficient.
  - Zach replied that he wants the algorithm in the paper so that the same behavior is exhibited on all platforms
    and is unconcerned about rendering dependent cases like for family emoji.
  - Victor reiterated that width calculation is best effort and that he is ok with consistent results only being
    ensured for the basic source character set.  This assurance only requires a fixed system dependent encoding.
  - JeanHeyd asked for clarification that we would only be guaranteeing alignment for the basic source character
    set in C++20 while leaving further specification until C++23.
  - Victor replied, yes, basically.
  - JeanHeyd asked if that implied an implementation defined fixed encoding.
  - Victor responded, not implementation defined, but rather platform dependent so that all implementations targeting
    a given platform would exhibit the same behavior.
  - Tom observed that, if the system defined fixed encoding differs by platform, then we won't get consistent results.
  - Zach disagreed based on a premise that, for the purposes of width computation, consistent results are achieved by
    interpreting the input as Unicode.
  - Corentin stated that he thinks we need to defer to (wide) execution encoding when computing width.
  - Tom agreed stating that we should make width calculation as right as we can make it.
  - JeanHeyd reformulated the trade off.  The most right answer depends on locale.  The always consistent result
    generates garbage consistently but avoids the locale dependency.
  - Victor stated that rendering can always change; we just need to decide if we are ok depending on something at
    run-time.
  - Zach re-iterated that, with the current specification, width calculation only works for single byte characters
    that render as a single glyph and we don't have a way to customize the width formatting unless we defer to
    something at run-time, but doing so conflicts with design goals of `std::format`.
  - Corentin observed that the same issue exists with `printf` as it will fail if the execution encoding doesn't
    match the run-time locale encoding; C and C++ fundamentally depend on encoding compatibility.
  - Victor reminded everyone that the paper does support use of the locale encoding via an opt-in specifier.
  - Steve reminded everyone that there is no system call to get the actual display width, so we're always guessing
    anyway.
  - JeanHeyd stated that he thought opt-in for locale dependent width was acceptable.
  - Zach expressed a desire to get the right default for the long-term.  If we make the default behavior locale
    sensitive, then we'll be stuck with that forever.
  - Tom responded that, in the long term, encoding will hopefully become separated from locale thereby eliminating the
    wrong default concern.
  - Corentin suggested that, for C++20, we could require the `'l'` in the specifier and not have a non-locale option
    until we figure this out.
  - Steve observed that the locale dependency creates a buffer overflow situation in the case where the locale changes
    in between width calculation and actual formatting to a buffer.
  - Corentin stated a preference to just require `'l'` in the width specification for C++20 to give us time to address
    this properly.
  - Tom suggested adding a reference to [LWG 3290](https://wg21.link/lwg3290) in the paper.
- Tom announced that the next meeting will be on October 9th.


# September 4th, 2019

## Draft agenda:
- Discuss Corentin's draft D1854R0 - Conversion to execution encoding should not lead to loss of meaning
  - https://cor3ntin.github.io/posts/encoding/D1854.pdf
- Discuss a few follow up items from
  [P1689, "Format for describing dependencies of source files"](https://wg21.link/p1689) following discussion in SG15.
  - Bikeshed "data". What do we call the code unit equivalent in path names?
  - Are we ok stating that JSON readers/writers are not allowed to apply Unicode normalization?
  - Are we ok with allowing a BOM (JSON doesn't permit one)?
- Is "execution character set" the right term for the run-time locale dependent encoding used by the character
  classification and conversion functions?

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - David Wendt
  - JeanHeyd Meneide
  - Nathan Myers
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- The meeting started off with a round of introductions for the benefit of new attendees.
- Discuss Corentin's draft D1854R0 - Conversion to execution encoding should not lead to loss of meaning
  - https://cor3ntin.github.io/posts/encoding/D1854.pdf
  - Corentin introduced the paper:
    - The basic idea is to avoid the meaning of the program silently changing in unintended ways due to lack of
      representation in the execution character set for a character in a character or string literal.
  - Zach asked if he hadn't previously signed up to write this paper.
  - Corentin explained that Zach signed up to write a paper about `u8string`.
  - Tom then proceeded to explain the wrong paper but succeeded at only further confusing himself.
  - Zach clarified that the paper he did sign up to write was to permit `uX"xxx"` string literals only when
    the execution encoding is a Unicode encoding.
  - Tom returned discussion to the paper at hand and noted that the paper only adds restrictions on ordinary
    and wide literals because restrictions are already in place for `u8`, `u`, and `U` literals.
  - Corentin demonstrated via godbolt.org that gcc rejects non-representable characters and that MSVC
    substitutes a `?`.
    - https://godbolt.org/z/kDwR1l
    - \[Editor's note: demonstration of MSVC's substitution of a `?` character requires adding the
      `/source-charset:utf-8` option to the MSVC command line in the above link.  Without that option, the
       UTF-8 encoded source is interpreted by the MSVC compiler as Windows-1252.\]
  - Corentin summarized that the goal is to standardize gcc's behavior.
  - Corentin stated that he was unsure if Microsoft would be willing to implement this outside of `/permissive-`
    mode since this might break existing code even though such code is already fragile and subject to breakage
    just by being compiled on a different system (with a different default execution character set).
  - Tom noted that by making this standard, if an implementor remains non-conforming, then users can complain if
    they want to.
  - Tom asked if there are any possible advantages to status quo.
  - Zach replied no, this just hurts portability.
  - Corentin observed that code can always be updated to use an escape sequence instead of an unrepresentable
    character.
  - Peter expressed concern about wide encoding because, on Windows, it is (or used to be) UCS-2, so emoji can't be
    represented.
  - Tom restated Peter's point; there may be cases where graceful degradation is ok.  E.g., losing emojis.
  - Peter reported testing gcc and found that, in wide encodings, characters outside the BMP were lost when printing
    to the console.
    - ```
      int main() {
          std::wstring s = L"\U0001f4a9";
          std::wcout << s;
      }
      ```
  - Tom suggested that this is due to a libstdc++ iostreams issue; wide characters are simply truncated when
    `std::wcout` writes them to stdout.
  - Corentin demonstrated that gcc rejects wide string literals with characters not representable in the wide
    execution character set as well.
  - Tom requested a quick walk through of the wording.
  - Tom suggested to update the paper to use stable names for the sections to be updated since numbers change.
  - Peter noted that, in section 5.13.3.8, the red text is missing strike through.
  - Corentin commented that, until writing this paper, he was not aware of multi-character literals.
  - Peter responded regarding a recent use case for them for a table driven switch handling approach:
    - ```
      uint32_t tableId = (table->Signature[0] << 24) |
                         (table->Signature[1] << 16) |
                         (table->Signature[2] << 8) |
                         (table->Signature[3] << 0);
      switch(tableId) {
          case 'APIC':
          ...
      }
      ```
  - Tom expressed some initial surprise to see the proposed wording changes for octal and hex escapes, but
    concluded that they make sense.
  - \[Editor's note: it would be helpful to add examples to the paper of code that would become ill-formed.\]
- Discuss a few follow up items from
  [P1689, "Format for describing dependencies of source files"](https://wg21.link/p1689) following discussion in SG15.
  - Bikeshed "data". What do we call the code unit equivalent in path names?
    - Tom introduced the naming concern.  [P1689R0](https://wg21.link/p1689r0) used the name "data" to refer to the
      sequence of individual elements of a path.  [P1689R1](https://wg21.link/p1689r1) changed the name to "code-units"
      following feedback in Cologne.  Do we want to suggest a different name given our stance on file names not having
      an associated encoding and, arguably therefore, no "code units"?
    - Corentin argued to not invest time in this discussion unless/until SG15 progresses the paper further.
    - Corentin also observed that user's won't see this name, so it doesn't really matter.
  - Are we ok stating that JSON readers/writers are not allowed to apply Unicode normalization?
    - Tom explained that this is no longer a concern.  in [P1689R1](https://wg21.link/p1689r1), code units are always
      explicitly specified.
  - Are we ok with allowing a BOM (JSON doesn't permit one)?
    - Corentin argued that we should follow the JSON specification.
    - Tom explained his understanding that allowing one doesn't violate RFC 8259 since the BOM limitations there only
      apply to network-transmitted text, and ECMA 404 doesn't specify encoding at all; there is no mention of "BOM",
      "byte order", or "UTF-8" in that specification.
      - https://tools.ietf.org/html/rfc8259#section-8.1
      - https://www.ecma-international.org/publications/standards/Ecma-404.htm
    - Zach asked what motivation exists for allowing a BOM.
    - Tom replied that it would be useful for non-ASCII based platforms like z/OS.
    - Peter added that it is useful for Windows as well since text files are likely to be interpreted as Windows-1252.
    - Corentin noted that Unicode recommends against use of a BOM.
    - Corentin stated that, if the specifications don't require UTF-8 encoded JSON, then we should specify that.
- Is "execution character set" the right term for the run-time locale dependent encoding used by the character
  classification and conversion functions?
  - Zach suggested asking core about this since it seems like we've just been using the wrong terms.
  - Steve noted that the existing wording is all old langauge pertaining to character sets, not necessarily encoding.
  - Tom stated that there was an email thread about this on the core and SG16 mailing lists and that the conclusion
    was that Steve and Tom should write a paper.  Steve has since done some work, but Tom hasn't.
  - Zach stated that we need someone to go through the existing wording and refine our understanding of it.
  - Tom agreed, and added that that is the paper to be written.  We use terms like "execution encoding" now that
    aren't defined in the standard.
  - Steve stated he would love to expose encoding details somehow.
  - Corentin asked if we want to change the names as they've been around a long time.
  - Steve stated he thinks it is worth tightening the specification without changing the intent.  Other than that
    we should state that the wide character encoding can be a variable length encoding.
  - Zach commented that clarifying terms in the standard is a good use of our time.
  - Corentin stated we should have different names for compile-time and run-time encodings and that wording should
    state requirements regarding their compatibility.
  - Steve asserted that some archaeology is necessary here as much of this wording was created when locales were
    being developed around the expectation that code worked with the "C" locale.
  - Peter observed that variable length encodings go back to at least GB2313 from the 1980s.
  - Steve noted that shift encodings go back to then too.
- Zach mentioned that he has a repository where he is working on several small papers.
  - https://github.com/tzlaine/small_wg1_papers
- Peter requested feedback on his slides for CppCon.
- Tom stated that the next meeting will be September 25th.


# August 21st, 2019

## Draft agenda:
- Discuss P1108, "web_view".  Our focus will be, unsurprisingly, character encodings and the use of iostreams with (presumably) UTF-8 data.
- Goals for WG14 in Ithaca (October 21st-25th).
- Goals for Belfast (November 4th-9th).
- Discuss a few follow up items from P1689, "Format for describing dependencies of source files", following discussion in SG15.
  - Bikeshed "data".  What do we call the code unit equivalent in path names?
  - Are we ok stating that JSON readers/writers are not allowed to apply Unicode normalization?
  - Are we ok with allowing a BOM (JSON doesn't permit one)?
- Is "execution character set" the right term for the run-time locale dependent encoding used by the character classification and conversion functions?

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - Hal Finkel
  - Hubert Tong
  - JeanHeyd Meneide
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Discussion of a draft of P1108R3 - web_view:
  - https://wg21.link/p1108r3 (link not yet active).
  - Hal introduces.
    - A protoype is available using wxWidgets:
      - https://github.com/hfinkel/web_view
    - There are a variety of ways we can provide graphical interaction within the standard.
    - This approach comes out of discussions with folks at Apple and Nvidia.
    - This approach outsources functionality to well used outside standards.
    - The basic idea is that system services already exist with different APIs that can be wrapped in a standard
      interface.
    - For security reasons, interactions should run out-of-process and the interface must therefore not be too fine
      grained.
    - There is a common subset of functionality among the various system services that provides a push/pull interface.
    - Constructing a `web_view` presents a window in which web content can be displayed and (Javascript) scripts can
      be run.
    - URI scheme extensions are supported by registering a (single) callback handler (per scheme).
    - Close handlers are supported by registering a (single) callback handler.
    - Interfaces are provided to request window close and to wait for window close.
    - An example of a dynamic page is available in the paper.
  - Hal provided a (successful!) live demonstration of the example from the paper.
  - Hal then provided an additional (successful!) live demo of an additional example.
  - Zach asked how C++ code can be invoked to update the displayed page.
  - Hal responded that interaction is enabled by registering a URI scheme handler callback via the
    `set_uri_scheme_handler` interface.
  - Tom asked if the interface is effectively append only.
  - Hal responded that it is based on a push model, so yes, requests update state.  The design supports both push
    (via `run_script`) and pull (via callbacks registered with `set_uri_scheme_handler`).
  - Zach stated that users will want the ability to route schemes to direct requests.
  - Tom suggested that routing can be implemented via the callback registered with `set_uri_scheme_handler`.
  - Corentin suggested using Web Sockets as well.
  - Hal responded that there are many examples where utility libraries would come in helpful.  For example, we
    probably don't want to do URI encoding and decoding, nor build interfaces using `std::format`.  We probably
    want JSON support libraries.  Such utility libraries should be proposed separately though.
  - Tom asked to clarify if `run_script` is for Javascript only and whether it would make sense for other languages
    to be supported.
  - Hal responded that it may be useful to specify the scripting language, like for Web Assembly.
  - Zach suggested that such support could always be wrapped in Javascript.
  - Zach acknowledged the elephant in the room by asking about the use of `std::string` in the interface.
  - Corentin stated that we should give the same advice as for 2D graphics; use Unicode everywhere and,
    specifically, UTF-8.  Supporting both UTF-8 and UTF-16 would complicate the interface.
  - Zach noted that the W3C recommends UTF-8 only.
  - Zach observed that for support of [RFC 39865](https://tools.ietf.org/html/rfc3986), encoding of URIs could
    be handled within the library thereby allowing all URIs to be provided in UTF-8.  The remaining interfaces
    could all take UTF-8 only as well, except, perhaps, for the window title.
  - Tom stated that, for the title, even if UTF-16 is eventually required, conversion from UTF-8 is loss-less.
  - Corentin suggested that URI escaping is complicated and that an interface for it should not be part of this
    proposal.
  - Tom asked if existing web view providers provide URI encoding services or if the implementation would be
    obligated to provide it.
  - Hal responded that some web view implementors just reject invalid URIs and that some others may not validate
    much for file handling.  It isn't clear how existing web view providers interpret input; they probably just
    assume UTF-8.
  - Hal asked that, if UTF-8 were required, would it be sufficient to indicate that by just using `std::u8string`
    in the interface.
  - Zach responded yes, though `std::u8string` doesn't enforce well-formed UTF-8, so it may still be necessary
    to explicitly specify a requirement for well-formed UTF-8 data.
  - Corentin asked if use of `char8_t` based types doesn't already ensure that.
  - Hubert responded no, we can't enforce well-formedness since programmers can always create `char8_t` arrays
    with non-UTF-8 data.
  - Zach suggested that we add blanket wording somewhere in the standard library specification stating that,
    for interfaces that use `std::u8string` in library functions, that behavior is undefined if data is not
    well-formed UTF-8.
  - Hubert stated that approach makes sense.
  - Hal, changing topics, asked for feedback regarding use of `std::ostream` in the URI scheme callbacks.
  - Zach asked if we have `char8_t` based streams yet.
  - Tom responded no.
  - Zach stated that we would want that to help ensure the data is UTF-8.
  - Hubert suggested that `codecvt` facets could be used to perform conversions.
  - Zach acknowledged and added that, if the programmer imbues a locale, it is up to them to make sure it makes
    sense.
  - Corentin asked if Hal had considered use of strings instead of streams?
  - Hal responded that a string based approach might make sense.  The benefit of the stream approach is that it
    allows partial writes and some of the lower level interfaces support that.
  - Tom, clarifying, stated that, within a callback handler, data written to the stream may start being processed
    by the web view before the handler returns.
  - Corentin suggested that we're going to have to provide `char8_t` based streams in C++23 anyway.
  - Tom agreed.
  - Hubert returned discssion to the earlier comments on blanket UTF-8 wording for `std::u8string`.  The place to
    add such wording is in [[res.on.arguments]](http://eel.is/c++draft/res.on.arguments); "each of the following
    applies to all functions ... unless explicitly stated otherwise".
  - Zach volunteered to draft that blanket wording.
  - Hal stated that we kind of broke UTF-8 hello world in C++20, but iostreams are weird for non-text data anyway.
  - Tom replied that it was already broken, but we certainly didn't make it any easier.
  - Hubert noted that localizations on iostreams currently require characters not in ASCII.  For example, monetary
    symbols like the Euro sign (€).
  - Hal noted that the URI scheme handler takes a constrained parameter, so overloads could be provided to handle
    strings and streams.
  - Hal stated that the next revision of the paper will include discussion about the URI scheme handler composing
    a string and returning it vs support for partial writes via iostreams or some other concept.
  - Tom suggested that there may be something in the Networking TS worth looking at.
  - Hubert suggested that something lower level in iostreams, like `std::streambuf`, might be worth looking at too.
  - Hal observed that `std::streambuf` has an associated locale.
  - Tom acknowedlged; that is where `std::codecvt` facets do their work.
  - Tom pondered whether we should ban `std::codecvt` facets on future `char8_t`, `char16_t`, and `char32_t`
    iostreams by making attempts to imbue such streams with such a facet an error.
  - Tom mentioned that we've talked about string builders in the past and this is a clear example where such
    builders could be useful; though `std::format` might just be that tool these days.
  - Zach observed that Beast and the like traffic in large ranges.  Perhaps some of those types would be useful here.
  - Corentin suggested that Web Sockets are a better solution.
  - Tom asked if it might make sense for the URI scheme handler to just use Web Sockets.
  - Hal responded with concerns about complexity; the underlying APIs aren't the same.
  - Hal stated that we will need to figure out the string vs stream interface as we want to avoid having to do
    unnecessary copies.  We don't want to motivate the interface based on not knowing how to print UTF-8 to streams.
    Responses are probably small, so strings are usually ok.  But encoded images might get pushed through these
    interfaces as well.
  - Zach asked how many URL scheme handlers can be active at a time; if we were reviewing for SG13, I would want to
    know how much data can get pushed.
  - Hal responded that the interface currently feels quick from a human perspective, but measurements of throughput
    haven't been done yet.
  - Hal followed up with some details of the prototype; wxWidgets has an unfortunate feature where all of the URI
    callbacks are called on the UI thread.  That isn't desired since a slow handler blocks the UI.  All of the
    underlying implementations support running handlers on non-UI threads.  The prototype needs to be changed to
    further explore that.
  - Hubert noted that an implementation could presumably host this as a single processs where the C++ code is the
    plugin, so we can't necessarily assume a thread model.
  - Hal responded that, on most systems, the straight forward implementation method has the UI driven by a thread
    in the same process, but the web content renderer code runs in a separate process driven by RPCs.  This will
    determine performance characteristics.
- Discussion of goals for WG14 in Ithaca (October 21st-25th):
  - JeanHeyd stated that he is planning to attend and to bring papers for:
    - \[nodiscard\]
    - Additional conversion functions for `char` and `wchar_t`.
    - Support for `C.UTF-8` as the default C locale.
  - Steve stated that the only thing that knows the encoding of `wchar_t` is the standard library and asked if any
    encodings other than UTF-16 or UTF-32 are used in practice.
  - JeanHeyd responded yes, AIX for Chinese locales uses Big-5.
  - Tom added that z/OS uses a wide EBCDIC.
  - Corentin asked what the motivation is for SG16 to add more conversion functions to C.
  - JeanHeyd responded that it allows C++ implmenentors to use features provided by C.
  - Tom suggested that it might be worth asking implementors what they would want and whether they would actually
    use C interfaces.
  - JeanHeyd acknowledged and stated he would ask.
  - Zach stated that such interfaces might be nice to have for C, but C interfaces can't achieve the performance
    that Bob Steagall demonstrated with his UTF-8 work.
  - JeanHeyd noted that, since these interfaces are based on NTBSs, they will need to check for null characters
    or know the string length ahead of time.
  - Zach suggested that, for performance, it may be worth only looking ahead 16 bytes at a time.
  - Tom stated that he is hoping to attend Ithaca and to bring papers for:
    - char8_t.
    - Make char16_t/char32_t string literals be UTF-16/32.
    - Named character escapes.
- Discussion of goals for Belfast (November 4th-9th).
  - Steve stated he would like to put together an initial pass at cleaning up terminology for encoding and
    character sets.
  - Hubert stated that he would be happy with SG16 bringing such a paper, but timing is bad for CWG given where
    C++20 is at.
  - Tom stated he would like to bring a paper to enable a portable method of specifying that source files are
    UTF-8 encoded.
  - JeanHeyd stated he is working towards getting funding to work nearly full time on the
    [P1629](https://wg21.link/p1629) standard text encoding paper.
  - Tom asked JeanHeyd what we can do to help prove the design works well in practice and suggested porting some
    project to it to demonstrate that:
    - the interface works and fits existing use cases.
    - that code is better.
    - that performance is retained or improved.
  - JeanHeyd responded that there are opportunities for a few checkpoints along the way.  For example, CppCon
    where a presentation is currently planned.
  - Tom asked for candidate projects that would be good for exercising the interface.
  - JeanHeyd responded that he had previously tried with a chat server and that a text editor would be a good
    choice.
- Tom confirmed that the next meeting will be on September 4th.


# July 31st, 2019

## Draft agenda:
- Cologne post-meeting discussion.
- Goals for WG14 in Ithaca (October 21st-25th).
- Goals for Belfast (November 4th-9th).

## Meeting summary:
- Attendees:
  - JeanHeyd Meneide
  - Mark Zeren
  - Peter Bindels
  - Tom Honermann
  - Zach Laine
- Discuss drafting guidance explaining our consensus regarding providing char/wchar_t, char16_t, and char8_t
  overloads in Cologne.
  - Tom introduced the need to discuss guidance by presenting poll results taken for three papers:
    - [P1030R2](http://wg21.link/p1030r2): std::filesystem::path_view:
      - `char` and `wchar_t` oriented interfaces should be provided that behave according to the
        `std::filesystem::path` specification in terms of encoding.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   3 |   2 |   0 |   4 |   2 |

      - `char32_t` oriented interfaces should be provided that behave according to the
        `std::filesystem::path` specification in terms of encoding.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   2 |   2 |   4 |   2 |   2 |

    - [P0267R9](http://wg21.link/P0267R9): A Proposal to Add 2D Graphics Rendering and Display to C++
      - Provide overloads for `char` (execution encoding) and `wchar_t`.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   0 |   0 |   4 |   3 |   3 |

      - Provide overloads for `char16_t`.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   0 |   0 |   5 |   2 |   3 |

      - Provide overloads for `char32_t`.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   0 |   0 |   3 |   3 |   4 |

    - [P1750R0](http://wg21.link/P1750R0): A Proposal to Add Process Management to the C++ Standard Library
      - Provide `std::process` `char` (execution encoding) and `wchar_t` interfaces.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   7 |   2 |   0 |   0 |   0 |

      - Provide `std::process` `char8_t` interfaces.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   3 |   0 |   5 |   0 |   0 |

      - Provide `std::process` `char16_t` interfaces.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   0 |   0 |   8 |   1 |   0 |

      - Provide `std::process` `char32_t` interfaces.

        |  SF |   F |   N |   A |  SA |
        | --: | --: | --: | --: | --: |
        |   0 |   0 |   3 |   5 |   0 |

  - Tom explained that, to an outside observer, our guidance looks inconsistent:
    - For polls about providing `char` and `wchar_t` based interfaces:
      - For P1030R2, we were evenly split with strong positions on both sides.
      - For P0267R9, we were fairly opposed to providing them.
      - For P1750R0, we were strongly in favor of providing them.
    - For polls about providing `char16_t` based interfaces:
      - For P1030R2, we didn't even ask the question (we know of UTF-16 based file systems).
      - For P0267R9, we were opposed to providing them.
      - For P1750R0, we barely could have cared less about the question.
    - For polls about providing `char32_t` based interfaces:
      - For P1030R2, we were evenly split with strong positions on both sides.
      - For P0267R9 and P1750R0, we were opposed (though more strongly so for P0267R9).
  - Zach addressed `char32_t` as the easy case first.  The `char32_t` overloads exist for completeness, but no one
    actually uses them.  They are inefficient.  `char32_t` is more useful for interfaces that accept non-contiguous
    data.
  - Mark stated that `char32_t` is useful when examining Unicode scalar values or elements of a grapheme cluster.
  - Zach replied that, If we have a grapheme cluster span like type some day, then we'll want a contiguous
    `char32_t` interface.  We can always add `char32_t` overloads as needed later.
  - Mark agreed that we can wait for use cases to materialize.
  - Tom asked if we should consider deprecating any existing `char32_t` interfaces.
  - Peter, despite not having been present for these polls and related discussion in Cologne, quickly recognized
    some patterns in the polls and offered some insightful rationale:
    - For P1750, we are replacing existing functionality, so need to support existing non-standard `char` and
      `wchar_t` based interfaces.  `char8_t` is our intended future direction, so we want that interface.  We don't
      want to emphasize `char16_t` and `char32_t` going forward.
    - For P0267, we are not replacing existing functionality, so we don't need `char`, `wchar_t`, `char16_t`, or
      `char32_t` based interfaces; we can restrict to `char8_t` for now.
    - For P1030, it seems like we don't know what we want.
  - Mark added an additional rationale for P0267; fonts are Unicode based, so it makes sense to just start with
    Unicode input.
  - Tom noted that, in the time since Cologne, Niall has decided to add `char` and `wchar_t` based interfaces to
    P1030.
  - Zach expressed support for Peter's observations; `char` and `wchar_t` based interfaces are important for migration
    purposes.
  - Mark agreed and noted that we don't want to construct road blocks for proposals for new interfaces.
  - Peter acknowledged that we don't want to make migration difficult and then raised the point that Apple's HFS+ and
    APFS filesystems are problematic for `path_view` because their behavior is non-portable.
  - Zach noted that similar problems exist for Windows with NTFS allowing UCS2 file names that are not valid UTF-16.
  - Peter provided an additional example regarding FAT derived filesystems storing locale case translation tables and
    noted that this is problematic when files are written with one locale and read using a different one (probably on
    a different system).
  - Tom returned to Peter's rationale in the context of P1030.  What is being proposed is a more performant alternative
    for some uses of `std::filesystem::path`.
  - Peter stated that the rationale for not providing `char` and `wchar_t` based interfaces is that the filesystem only
    offers bytes when names are enumerated.  If we give those bytes back, the filesystem will accept them.  We can get
    a displayable string, as from the `u8string()` member function of `std::filesystem::path`, but we can't necessarily
    pass that path back to the filesystem.
  - Tom stated that that rationale contradicts guidance regarding not wanting to construct impediments to migration.
    The vast majority of file names use only the basic source character set.  By not providing `char` interfaces, we're
    making very common use cases difficult.
  - Zach observed that support for all valid file names requires use of `char` on Linux and `wchar_t` on Windows today.
    The goal of the `std::byte` oriented interface is to provide something portable.
  - Tom objected to those interfaces providing a portable abstraction since:
      1. The underlying operating system interfaces used to implement those interfaces may themselves perform
         translations.  For example, the normalization performed by HFS+ and APFS, and
      2. Some OS interfaces don't support arbitrary byte sequences as file names.  For example, on Window's, a byte
         oriented interface would either use `CreateFileA` which would perform locale conversions, or `CreateFileW` which
         requires a sequence of 16-bit values (e.g., an odd number of bytes isn't supported).
  - \[Editor's note: at this point, Tom became completely engrossed in the conversation and utterly and completely
    failed to record individual commentary.  The following reflects his recollection of the discussion.
      - Zach lol'd at the contortions that Tom's face apparently exhibited as Tom struggled to comprehend why anyone
        thought the `std::byte` based interface was a good idea.
      - Tom was awakened to the possibility that the `std::byte` interface wasn't necessarily conceived of as a means to
        specify an actual sequence of bytes to be stored directly in the filesystem, but rather as a pointer to a sequence
        of bytes that represent an opaque structure that was (probably) provided by the OS in the first place.

    \]
  - Zach stated that `path_view` is intended for performance and doesn't support mutation.
  - JeanHeyd asserted that the `std::byte` oriented interface is intended to allow passing back to the OS a path name
    that was originally provided by the OS.
  - Zach agreed and added that the byte oriented interface is more like a handle to a file name, specifically a reference
    to something matching the representation stored in `std::filesystem::path`.
  - JeanHeyd added that the byte oriented interface exists for performance, but the `char` and `wchar_t` interfaces should
    be provided for simple portable uses.
  - Zach expressed a preference for making use of the `path_view` `char` based interface ill-formed on Windows and use of
    the `wchar_t` interface ill-formed everywhere else, but added he was now convinced that the `char` and `wchar_t` based
    interfaces should be provided.
  - Mark observed that providing those means we need to worry about life-time management and when conversions occur.
  - JeanHeyd responded that working implementations of `path_view` have already shipped and have demonstrated reduced
    overhead due to avoidance of allocation.
  - Tom expressed a preference for introducing a `raw_path` type to represent a canonical path rather than using
    `std::byte`.
  - JeanHeyd suggested using `std::filesystem::path::value_type` but noted that casts would still be needed.
  - Zach ponded the idea of a `raw_path` type that is only constructible from `wchar_t` on non-Windows systems and only
    constructed from `char` elsewhere.
- Tom confirmed the date for our next telecon; August 21st with the intent being to discuss [P1108R2](http://wg21.link/p1108r2) - `web_view`.


# June 26th, 2019

## Draft agenda:
- Discuss papers from the Cologne pre-meeting mailing.  At least:
  - P1629R0 - Standard Text Encoding
  - P0267R9 - A Proposal to Add 2D Graphics Rendering and Display to C++
    - just the new interfaces for text rendering.

## Meeting summary:
- Attendees:
  - Elias Kosunen
  - Hubert Tong
  - JeanHeyd Meneide
  - JF Bastien
  - Mark Zeren
  - Michael Spencer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Tom started the meeting with some administrative details:
  - Our regular meeting cadence would have us meet July 10th and July 24th, but the Cologne meeting is the 15th through
    the 20th.  Tentative plan is to skip the next two regular meetings, meet July 31st, and then back to our regular
    meetings during the 2nd and 4th weeks of the month in August.
  - Hubert asked when the post-meeting mailing deadline is.
  - Mark responded, August 5th.
  - Tom communicated that issue #8 (https://github.com/sg16-unicode/sg16/issues/8) has been closed as resolved by the
    adoption of [P1139R2](https://wg21.link/P1139R2) in Kona.
  - Tom also communicated that the revision of [P1423R2](https://wg21.link/p1423r2) in the Cologne pre-meeting mailing
    adds deleted `operator<<` overloads for wide streams for `char8_t`, `char16_t`, and `char32_t` following LWG
    feedback during their [May 21st paper review telecon](http://wiki.edg.com/bin/view/Wg21cologne2019/LWGTelecom21May).
    These changes will require LEWG review in Cologne.
- [P1629R0 - Standard Text Encoding](https://wg21.link/p1629r0):
  - JeanHeyd presented and provided a link to a draft revision (with only clerical errors fixed).
    - https://thephd.github.io/vendor/future_cxx/papers/d1629.html
    - The proposal includes low level and high level interfaces.
    - Normalization support will come later.
  - Peter (via chat): There is a typo in section 3.3.2, "GB1032" should be "GB2312" or "GB18030".
  - Elias (via chat): In 3.2.3.2, on the last line of the first snippet, the `basic_utf8` instead of `basic_utf16`
    is probably a typo?
  - Zach expressed surprise at the lack of low level transcoding algorithms and lack of iterator based interfaces.
  - JeanHeyd replied that those algorithms are implemented within the encoding object and that the interface is
    range based rather than iterator based.  Objects are used instead of free functions in order to maintain state.
  - Zach asked where code point conversion is happening; there isn't much state needed.  
  - JeanHeyd explained that roundtripping through the encoding handles code points internally.  State is needed for
    non-Unicode encodings and for error handling.
  - Zach stated that, in Boost.text, the error handler is a template parameter.
  - Zach asked if this design precludes doing performance optimizations like Bob Steagall has demonstrated.
  - JeanHeyd replied that such optimizations are excluded in the encoder interface, but are intended to be supported
    by specializing the high level interfaces; the specified free functions are customization points that can enable
    optimizations.
  - Tom asked why the `encode` and `decode` functions on the encoding object preclude optimizations.
  - JeanHeyd replied that they only process one code point at a time.
  - Zach asked what the motivation is for the slower interfaces over faster ones.
  - JeanHeyd replied that the `encode` and `decode` customization points are eager and convert as much as possible.
    The encoding object enables an iterative approach in which writing just the encoding object suffices to enable
    the high level interfaces to work correctly, but at a less-than-optimal speed.  
  - Steve said that it sounds like the code point at a time encoding object is the extension point for custom encoding.
    It is unlikely that anyone will bother with a high performance implementation for many legacy encodings as
    vectorizing support takes a lot of work.
  - Zach expressed support for a convenience approach and a fast path, but also sees value in an iterator approach as
    well.  Encoding details should be in either the algorithm (eager/fast) or in the iterator (lazy/slow).  Having
    building blocks for constructing iterators isn't key.
  - Zach expanded by contrasting with Python where the encode and decode functions always confused him because encoding
    and decoding are basically different names for the same algorithm with direction reversed.  This design seems over
    generalized.
  - Tom stated that the design is range based, so iterators can be wrapped in a range, does that not suffice for
    iterator use cases?
  - Zach replied that standard alorithms don't take an output range, they take output iterators.
  - Zach stated, when I'm doing a transcode, sometimes I want to loop and break, sometimes I just want to convert
    everything.
  - Peter stated he was confused by Zach's comments.
  - JeanHeyd attempted to paraphrase.  What Zach is saying, is rather than specify building blocks, we should specify
    lazy transcoding iterators.  The concern with that approach is that writing an iterator is a lot harder to do.
  - Tom agreed noting that he discovered how hard they are to write when working on text_view.  For example, decoding
    iterators need to eagerly consume code units.  
  - Mark noted that we don't need to make it easy for implementors to write iterators, but it is good to make things
    easy for other programmers.
  - Zach stated that someone still needs to write the lazy iterator.  There is an impedence mismatch between input
    and output.  A general template based iterator doesn't work.
  - Tom stated it did for text_view.
  - JeanHeyd stated that the ideas came from text_view and libogonek.  The encoding object avoids having to write
    iterators and ranges.
  - Zach stated he would like to understand how that works.
  - Tom explained how input text iterators and output text iterators can be used together; e.g., via `std::copy`.
  - JeanHeyd expounded; Libogonek proved this out and Peter's S2 library did something similar.
  - Peter (via chat): +1, doing exactly that in http://github.com/dascandy/s2.  I have the rope concept that
    combines different code-point iterators as a single range so you can copy from that to a target (and the
    assignment operator for target encodings is optimized to first calculate size & then do the copy).  
    `s2::basic_string<s2::encoding::utf8> u8s = u16s.view();`
  - Peter (via chat): 90% sure this is my hook for encoding conversion fast path -
    https://github.com/dascandy/s2/blob/master/include/s2/detail/rope_detail.h#L41
  - Zach said he would like to see the code in libogonek to better understand it.  It is well understood how encoders
    produce code units and decoders produce code points, but hard to see how transcoding can be done without missing
    optimization opportunities.
  - JeanHeyd explained that the fast path customization points enable that optimization by skipping the separate
    decode and encode steps.
  - Zach asked if iterator facade ever got standardized?  It makes writing iterators easy.
  - \[Editor's note: no they haven't.  The iterator facade proposal is [P0186](https://wg21.link/p0186).  It was
    discussed in Oulu in 2016.  Meeting minutes are [here](http://wiki.edg.com/bin/view/Wg21oulu/P0186).\]
  - Zach expressed skepticism regarding encoding builders; we just need to worry about common encodings.
  - Tom stated that there are use cases for code point at a time enumeration.
  - Zach agreed but stated that should be provided via lazy iterators; this design is taking generic programming too far.
  - Zach expressed a desire to be able to write a transcoding iterator that avoids construction of the intermediate
    code point value during conversion.
  - JeanHeyd noted that there are three extension points for customizing performance: the encoding object, transcoding
    iterators, and customization points.
  - Steve provided an example in which fast transcoding is trivial: transcoding ASCII to ISO-8859-1.
  - Mark observed that programmers want fast functions and transcoding iterators, not encoding objects. 
  - Steve stated that, within iconv's implementation, all transcoding conversions go through Unicode code points for
    all encodings.  This is presumably fast enough for most use cases.  Converting from Shift-JIS to Big-5 doesn't
    require extreme performance.
  - JeanHeyd stated that additional work is needed to enable that middle path with fast transcoding iterators.
  - Tom agreed; we need the lowest level for fall back to enable transcoding iterators between all encodings, but
    can optimize specific cases.
  - Zach stated that we really just need to list the specific transcoding iterators that are required.
- [P0267R9 - A Proposal to Add 2D Graphics Rendering and Display to C++](https://wg21.link/p0267r9):
  - Tom, unsurprisingly, stated that the interface should use `std:u8string` since it requires UTF-8 encoded text.
  - Michael agreed and expressed dislike for the asumption of UTF-8 in a `std::string` object.
  - Zach stated that the interfaces should be `std::string_view` and execution encoding.
  - Steve pondered whether all current graphical display systems are Unicode.
  - Tom stated that the X window system is locale based.
  - Zach suggested it would be least surprising to programmers to use execution encoding.  That way they can just
    pass regular strings.
  - Peter stated that, On UNIX systems, UTF-8 tends to be the default, so things will work as is, but Windows
    would be problematic.
  - Zach observed that, without standard library support, converting text from execution encoding to UTF-8 is hard.
  - Peter suggested leaving it to the UI libraries to figure it out.
  - Zach responded that this is a UI library, so we need to figure it out.
  - Michael pondered whether we should add overloads for `char`, `wchar_t`, `char8_t`, `char16_t`, and `char32_t`.
  - Zach suggested that we only need `char` and `char8_t`.
  - Hubert observed that the standard library is designed around locales.
  - Tom asked Hubert to clarify, are you thinking these interfaces should take a locale object?
  - Hubert responded that, if you have strings that you don't know the encoding for, then yes.
  - JeanHeyd expressed a preference for just using `std::u8string` to avoid locale dependencies.
  - Mark agreed that, perhaps, just `char8_t` is enough.
  - Tom stated that, by the time 2D graphics is standardized, we should be able to get good conversion routines in
    the standard library or we will have failed miserably!
  - Hubert observed that the paper is missing bidirectional language support.
  - Tom noticed that the paper doesn't say what happens with ill-formed encoded input.
  - Mark suggested discussing font names; these should probably be bag-of-byte names.  The paper defers to the HTML
    CSS specification.
  - Zach noticed that the paper doesn't discuss normalization.  It would be nice if it called it out specifically.
  - Tom asked if normalization matters.
  - Zach responded that it does in some cases.
  - JF suggested that we should make it possible to defer to the CSS specification if we can't right now.  We don't
    want to do what we previously did in forking the Unicode identifier specification from
    [UAX#13](https://unicode.org/reports/tr31)
  - Mark noticed that some of the interfaces pass and return `std::string` by value where they probably shouldn't.
  - JF pondered about overlap with SG13 and avoiding conflicts in scheduling when meeting in Cologne.
  - \[Editor's note: SG13 and SG16 are meeting on separate days.\]
- [P1750R0 - A Proposal to Add Process Management to the C++ Standard Library](https://wg21.link/p1750r0):
  - Elias described the overlap with [P1275](https://wg21.link/p1275) and stated he is aware of previous SG16
    review and is working with Isabella Muerte.
  - Elias described the pipe interface.
  - Tom asked if any operating system supports wide pipes.
  - Elias stated he is unsure if Windows does.  The interface is templated on char type.
  - Tom stated that Windows doesn't; `ReadFile` and `WriteFile` are used with pipes and they are byte oriented.
  - Hubert asked about the interaction with streams.
  - Elias responded that pipes can be wrapped in iostreams.
  - Tom summarized the feedback so far: wide pipes may not be needed and prior SG16 concerns regarding environment
    variables still stand.
  - Tom stated that command lines probably need to be considered to be in execution encoding.
  - Hubert stated that, for command lines, `exec` interfaces will likely be used and they use arrays, not strings.
    A formatting approach makes sense.
  - Elias stated that `process_launcher` takes a `std::filesystem::path`, not a string.
- Meeting in Cologne.
  - Tom communicated the tentative schedule for when SG16 would meet.
  - Zach stated he will miss Monday.


# June 12th, 2019

## Draft agenda:
- Discuss and provide feedback for any draft papers targeting the 6/17 pre-Cologne mailing.

## Meeting summary:
- Attendees:
  - Nathan Myers
  - JeanHeyd Meneide
  - Mark Zeren
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Planning for Cologne:
  - Tom communicated that SG16 has requested a half day session in Cologne.
  - Tom communicated that SG16 will host an evening session.  Potential topics (subject to author's desire) include:
    - UTF-8 and current ecosystems.
    - JeanHeyd's work on transcoding interfaces.
    - Corentin's work on character properties.
    - Hana's work on Unicode support in CTRE.
  - JeanHeyd confirmed that his transcoding interfaces paper will appear in the pre-meeting mailing.
- Discussion of the file name constraints added to the draft D1238R1 posted to the SG16 mailing list:
  - http://www.open-std.org/pipermail/unicode/2019-June/000386.html
  - Steve expressed approval for the new section.
  - Zach agreed noting uncertainty that anyone cares about the details of normalization-insensitivity.
  - Tom concurred and indicated he was unsure how important that is.
  - Zach stated that it is important since extremely subtle bugs can happen from changing normalization.
  - Tom acknowledged the possibility and noted reported problems for Apple's migration from HFS+ to APFS.
  - Zach observed that there is no good way to tell what filesystem you are working on and what its
    idiosyncracies are.
  - Nathan asserted that programmers have to deal with presentation of file names and allow user selection.
  - Steve noted that different file names can present the same (due to Unicode confusables or normalization
    differences).
  - Zach recalled an email from Marshall Clow some time ago regarding file systems using completely different
    normalization schemes.  Different filesystems do things differently.
  - Nathan stated that uploading a file to a web site also has presentation issues.
  - Mark stated that jumping from one filesystem to another is inherently lossy, but treated as a transfer issue.
    The only way to store a file accurately in text is to write it in something like base64.  Writing a file name
    to a text file may break the encoding of the file.
  - Zach claimed that we can't fix these issues except by declaring "things must work" and letting implementors
    figure it out, which they probably can't do.
  - Steve noted that we keep getting asked about handling of file names and this is intended to document constraints.
  - Mark recalled an example; from the stack trace proposal, we specified file names be handled as a sequence
    of bytes.
  - Tom mentioned he was thinking about sending an email to the Unicode Consortium's mailing list asking about
    current thinking regarding file names in text files.
  - Mark argued that we should just try and stay out of this space.
  - Tom asserted it is a big question for `std::text`.  How do we allow file names in `std::text`, particularly
    if we require well-formed content?
  - Mark suggested relying on an error policy.
  - Zach claimed that we need to emphasize that, if a file name is retrieved from the file system, programmers must
    maintain it as is.  Don't mutate it at all, don't compare it to text.
  - Tom asked how one puts file names in text and have it be well-formed text?
  - Zach replied simply, you don't.
  - Mark provided an example of Apache using base64 encoding of names in URLs.
  - Zach asserted that applications must provide a file selector interface.
  - Tom asked how one would write `ls`?
  - Zach responded that the file name be written in a presentation format that isn't necessarily suitable for
    referencing the file.
  - Steve observed that this already happens all the time that file names appear in output, but can't be parsed out
    or referenced as is.
  - Tom acknowledged and observed this is why GNU `find` has a `-print0` option.
  - Nathan suggested that we may need to publish a document on how to deal with file names.
  - Steve mentioned that we have `std::filesystem` and it has facilities for getting names out of paths.
  - Zach claimed that problems happen if, for example, you have a UCS-2 file name on Windows that is ill-formed
    UTF-16.
  - Tom confirmed that recent Windows 10 releases still allow creation of file names that are not valid UTF-16.
  - Zach asserted that we don't want interfaces that do transcoding or normalization to touch filenames.
  - JeanHeyd suggested adding a new non-directive to the paper stating that we won't attempt to impose restrictions
    on file names.
  - Tom agreed to do so.
  - \[Editor's note: Tom did so in [P1238R1](https://wg21.link/p1238r1) for the Cologne pre-meeting mailing\]
- Discussion of planned transcoding papers:
  - Zach stated he wasn't going to be able to produce a paper on transcoding for the Cologne pre-meeting
    mailing.
  - Tom let Zach know that was ok, especially since JeanHeyd was who had volunteered to write that paper and is
    currently working on a draft.
  - Zach noted a performance concern to address in the paper; generic transcoder interfaces don't perform well
    with smart iterators.  For maximum performance, vector operations must be used as Bob Steagall demonstrated.
  - Tom acknowledged that specializations for contiguous storage are needed.
  - JeanHeyd said he came to the same conclusion and that the paper would discuss it.  He also indicated intent
    to share the draft on the SG16 mailing list.
  - \[Editor's note: JeanHeyd later shared that draft on the SG16 Slack channel.  The draft can be found at
    [https://thephd.github.io/vendor/future_cxx/papers/d1629.html](https://thephd.github.io/vendor/future_cxx/papers/d1629.html)
    and will be in the pre-meeting mailing as [P1629R0](https://wg21.link/p1629r0).\]
- Discussion of z/OS compiler updates:
  - Tom communicated recent news within the z/OS ecosystem.  IBM recently released versions of Clang for z/OS with
    their latest updates for their xlC compiler.  Additionally, a third party provider also maintains a z/OS
    C++14+ compiler based on LLVM.  Tom stated the details would appear in a revision of P1238.
  - \[Editor's note: Tom did add those details to [P1238R1](https://wg21.link/p1238r1) for the Cologne pre-meeting
    mailing\]
- Discussion of Boost review for JeanHeyd's [`out_ptr`](https://github.com/ThePhD/out_ptr).
  - Zach communicated that Boost formal review for JeanHeyd's
    [`out_ptr`](https://github.com/ThePhD/out_ptr) library would begin on Monday June 17th and encouraged everyone
    to participate via the Boost mailing list.
- Discussion of C standard string transcoding functions:
  - JeanHeyd asked for feedback regarding a set of transcoding functions he is considering proposing to the C
    committee at their October meeting in Ithaca.  The functions match the existing C `mbstowcs`/`wcstombs` and
    `mbsrtowcs`/`wcsrtombs` functions but transcode between UTF-8 (`char8_t`), UTF-16 (`char16_t`), and UTF-32
    (`char32_t`).  The full cartesian product for all of the encodings results in approximately 40 functions.
    He is wondering if the full set is needed or if a reduced set would suffice.  These functions are not used
    often and aren't very performant.
  - Tom stated that `mbstowcs` should be able to perform ok.
  - JeanHeyd stated that dropping the restartable ones would reduce the number, but those are useful in some cases.
    Another approach is to just propose the `c8`, `c16`, and `c32` variants that convert between the execution
    encoding.
  - Tom agreed that just providing conversion between the execution encoding and UTF variants was probably sufficient.
- Discussion of updates to [P1072](https://wg21.link/p1072):
  - Mark provided an update regarding plans for [P1072](https://wg21.link/p1072).  There are two options:
    - Propose a lambda based interface.
    - Propose an independent class coupled to `std::string`.
  - Mark clarified that neither proposal would appear in the pre-meeting mailing.
  - Zach expressed a desire for the functionality and for additional progress to be made.
- Discussion of planned C committee proposals:
  - Tom asked for any volunteers interested in writing and presenting papers to the C committee in October that
    propose functionality we've added or plan to add for C++.  Such features include:
    - `char8_t` ([P0482](https://wg21.link/p0482))
    - Make char16_t/char32_t string literals be UTF-16/32 ([P1041](https://wg21.link/p1041))
    - Named character escapes: ([P1097](https://wg21.link/p1097))
  - JeanHeyd asked if we had ever followed up with the C committee regarding any known implementations that use
    an encoding other thatn UTF-16/UTF-32 for `char16_t`/`char32_t` literals or that don't define
    `__STDC_UTF_16__` and/or `__STDC_UTF_32__`.
  - Tom responded that Philipp Krause had confirmed that there are no known implementations.
  - \[Editor's note: though not mentioned in the meeting, there are implementations that use UTF-16 and UTF-32,
    but neglect to define the `__STDC_UTF_16__` and/or `__STDC_UTF_32__` macros.\]


# May 22nd, 2019

## Draft agenda:
- Axel Andrejs from Microsoft will present and discuss Microsoft's ongoing efforts to improve UTF-8 support in Windows.

## Meeting summary:
- Attendees:
  - Axel Andrejs
  - Henri Sivonen
  - Hubert Tong
  - JeanHeyd Meneide
  - Mark Zeren
  - Steve Downey
  - Tom Honermann
- Microsoft's on-going efforts to improve UTF-8 support in Windows.
  - Axel lead with some Windows history and current efforts:
    - Windows was initially developed using locale dependent code pages, but switched to UTF-16 long ago.
    - Today, the industry is moving to UTF-8, especially on the web.
    - So, what to do about UTF-8?  Initial efforts were to improve resource managers, but Windows remains UTF-16 based.
    - Now, what about the rest of the OS?  Windows 10 added support for UTF-8 as an (algorithmic) active code page (ACP).
    - All of Windows' "ANSI" interfaces use `MultiByteToWideChar()` to convert `char` based input to UTF-16 using the
      active code page.  This suffices to make UTF-8 mostly magically work, though Microsoft platforms remain UTF-16 based.
    - Within the industry, most Windows developers don't test support for many code pages; usually just a few for
      critical markets.  This leaves a large testing gap.
    - UTF-8 poses some problems as a code page.  Testing UTF-8 support has revealed cases of code that fails due to a
      number of issues:
      - It is variable length and may require more than two code units per code point.  Code that (incorrectly) assumes 
        wide strings are always larger (in terms of bytes) than the corresponding narrow encoding can cause buffer
        overflows.
      - The native C and C++ character type is `char` which is good in terms of flexibility, but bad in terms of type and
        encoding safety.  `char` can be used for UTF-8, but failure to perform conversions where needed leads to mojibake.
      - Some code just plain fails with any non-ASCII characters.
      - Many programs assume an encoding (ASCII or Windows-1252) and fail with UTF-8.
    - Because of known cases of existing code failing when the active code page is changed to UTF-8, Microsoft has no
      plans to ever change an existing machine's active code page.  There is too much potential for breakage.
    - Current efforts include improving UTF-8 support for individual Windows components, resource managers, resource
      loaders, etc...  UTF-8 strings tend to be approximately 1/3 the size of wide strings on average.  Focus on where
      that savings is beneficial.
    - UTF-8 as active code page is still a beta option because there are major applications that don't work correctly
      when it is enabled.  Most applications work ok in the US, but have problems elsewhere.
    - Outside Windows desktop where compatibility with legacy applications is less important, Windows platforms are
      moving more towards UTF-8.  These include Xbox, Hololens, smart devices, etc...
    - Bifurcation will continue.  We may evangelize UTF-8 in some markets, but are unlikely to ever be able to move the
      entire Windows ecosystem to UTF-8.
    - The latest Windows 10 release allows executables to opt-in to UTF-8 as active code page via a fusion manifest.
    - May add support for executables to opt-out of UTF-8 support for compatibility, either via a manifest setting or
      application compatibility shim.
    - Windows subsystems will continue to migrate to UTF-8.
    - Would like to discontinue the ANSI/Wide interface split.  Likely to introduce more interfaces that are UTF-8/Wide
      or just UTF-8.
    - There are no plans for a native Windows UTF-8 kernel.
    - We'll continue to reach out to major applicaions that are found not to work correctly when the active code page is
      set to UTF-8.
    - Within Microsoft, a number of developers have UTF-8 enabled as the active code page on their workstations.
    - We will continue to do more ecosystem outreach as our confidence in UTF-8 as active code page increases.
    - But, Windows will always need to retain compatibility.
  - Henri asked if there are plans to augment existing ANSI/Wide interfaces with U8 variants that only work with UTF-8.
  - Axel responded that they would like to do so where it makes sense.  Decisions to do so are up to component owners.
    Feedback and requests for specific interfaces are appreciated.  There are no plans for mass conversion.
  - Henri asked what would happen if an executable that was marked to run with UTF-8 was run on an older platform.
  - Axel replied uncertainly that the fusion manifest entries would probably just be ignored.  This is what happens with
    Universal Windows Programs (UWP) support.  Unsure if there is a way to mark the executable to fail in such cases,
    but the executable could be marked to require a particular level of OS support.
  - Mark asked how an executable that opts-in to UTF-8 as ACP would interoperate at the command line.  Is any transcoding
    performed for stdin/stdout?
  - Axel responded that, at present, all the opt-in does is override the ACP, so no, no transcoding of the command line
    or standard streams is performed.
  - Mark observed that this can then lead to failures.
  - Axel affirmed adding that thought has been put into implicit transcoding, but it is a hard problem.
  - Tom agreed noting that file names pose a significant problem since they may not be representable in a particular
    encoding.
  - Axel acknowledged, but stated that most file names are UTF-16.
  - Henri asked about the new terminal coming to Windows 10.  How will it know how to interpret the output of a particular
    program?
  - Axel stated that there are active discussions about this and decisions are not yet settled, but people are working
    on it.
  - Tom asked about recommendations for current developers.  How do we move into this new world?
  - Axel responded that `char*` is kind of nice for it's genericity but isn't safe.  Stronger types add safety, but
    increase the interface surface area.  `char8_t` is a big topic.  ICU supports both `char16_t` and `wchar_t`, but
    adds surface area.  As a global model, that doesn't work too well.  If targeting Windows only, best to stick to wide
    strings.  For cross platform, we're all on this journey.  Would like more feedback.  There are always workarounds
    because developers can do conversions themselves.  If there is demand, we'll add additional interfaces if the value is
    there.  Would like more support for command line handling.  Really want the industry to move to UTF-8 as ACP.  Library
    writers have to worry about all code pages anyway, including now UTF-8.
  - Mark asked how new types like `char8_t` fit in.
  - Axel expressed similar curiosity.
  - Tom provided some thoughts on `char8_t`.  Unsure what kind of adoption will occur.  Expecting to see uses in niches or
    as an internal encoding type.  Type safety can be used to guard components that are UTF-8 only from components that are
    locale dependent.
  - Henri asked if type based alias analysis is planned for the Microsoft compiler.
  - Axel responded that he was unsure of the compiler team's plans.  Windows interfaces have pretty basic types, so there
    is a lot of targeting lowest common denominator.  Always looking to take advantage of new features.
  - Mark offered the idea of a compiler option that would allow use of `char8_t`, but would mangle it the same as `char`
    for compatibility with prior compiler versions.  This would require errors for ambiguous overloads, but might still
    be useful.
  - Tom expressed interest noting that Microsoft already does something similar to duplicate symbols for `char16_t` and
    `wchar_t`.
  - Axel confirmed noting that they sometimes put code in headers and compile it twice (e.g., for ANSI and UNICODE
    expansions of `TCHAR`).
  - Henri asked why `char16_t` was not made the same type as `wchar_t` on Windows.
  - Hubert responded that C++ requires different types for overloading purposes.  Also because the encoding can differ.
  - Henri pondered whether, in retrospect, it would have been better if `char16_t` was specified as the same type as
    `wchar_t` on Windows and `char32_t` the same type as `wchar_t` on POSIX systems.
  - Steve stated that anyone attempting to write portable code is unhappy with `wchar_t`; it just isn't portable.
  - Mark added that the type separation is useful.  For `char8_t`, the non-aliasing properties are a good motivator for
    a separate type.
  - Steve concurred noting that injecting Unicode into the type system will be useful.  Additionally, `std::byte` will
    help us move further away from `char` for everything.
  - Axel mentioned that, on Windows, there are few APIs that take `char*` and that expect an encoding other than the ACP.
    The only indication is in documentation; the type system can't help enforce encoding expectations today.
  - Mark asked what code page is used for Windows Subsystem for Linux (WSL).
  - Axel responded that he would have to check, but once code reaches the kernel, everything is UTF-16.
  - Tom observed that different encoding expectations in Windows vs the WSL makes piping data problematic.
  - Mark surmised that the WSL uses UTF-8 like most Linux distributions.
  - Axel added that the International Platform team didn't do anything special for WSL.
  - Mark speculated that, if we decided to be very aggressive, we could require C++ code on Windows to run with the UTF-8
    manifest option.
  - Axel confirmed, but noted that requires new OS versions.
  - Tom asked Axel if his team had reached out to other language maintainers like for Python, Ruby, and Go.
  - Axel responded yes for .NET languages obviously, but not for other languages.  Need to reach critical mass internally
    first, and then will expand outreach to other languages.
  - Henri asked about enabling app development with UTF-8 on older OS versions.  Are there any plans for the standard
    library to provide UTF-8 interfaces that convert to internal UTF-16 ones?
  - Axel responded that work is progressing to improve support for UTF-8 in the CRT, but not sure of the time line.
  - Mark asked about any work on interfaces that operate at the grapheme cluster level.
  - Axel responded no, at present, they are more focused on basic APIs like `CreateProcess`.
  - Tom asked how SG16 can help with the effort to improve UTF-8 support.
  - Axel explained that the big challenge is how to handle the lowest common denominator.  New language features are used
    internally, but public APIs are very old school and limited to basic types.
  - Tom clarified, so keeping interfaces indpeendent of fancy new language features is helpful?
  - Axel responded yes, but always interested in new types that make sense within the Windows type system.
  - Tom summarized all of the different encodings that the C++ standard has to interact with; source encoding, internal
    compiler encoding, presumed (compile-time) execution and wide execution encodings, (run-time) execution and wide
    execution encodings, UTF-8, UTF-16, and UTF-32.  How do all of these encodings affect you?
  - Axel responded that they sometimes have to guess about encodings; may rely on BOMs or recognition of UTF-8.
  - Tom asked if Microsoft had ever considered allowing filesystems to support tagging files as having a particular
    encoding.  Some other OSs like z/OS have such support.
  - Axel responded no, not aware of any such efforts.
  - Mark asked if Windows makes use of the Unicode Private Use Area (PUA).
  - Axel responded yes, because, given the size of the development team at Microsoft, the answer to "do we use ..." is
    always yes somewhere.
  - Henri commented that Microsoft's eudcedit.exe editor generates a magic font for the PUA.
  - Tom asked if Axel had any thoughts about changing the default "C" locale to be UTF-8.
  - Axel responded that it might work out ok.  Old CRTs still get used though.  But this would make it easy for
    programmers to start using UTF-8.
  - Mark noted that Microsoft maintains backward compatibility, but that there may be some desire or intent for an ABI
    break at some point.  Perhaps that would be the right time to change the default locale encoding.
  - Henri asked if layering new versions of C++ on top of older C and C++ run-times is supported or whether new C++
    language standards require the latest C and C++ run-time libraries.  Would it be possible to have the compiler set
    the per-binary UTF-8 flag depending on target language level?
  - Axel responded that the UTF-8 flag affects the process, so can't depend on DLL options or settings.
  - Mark brought up a new issue; at some point, we will require various Unicode data sets.  Windows now distributes a
    version of ICU.  Concerns about the size of the chrono library were raised when it was added to the standard and it
    is much smaller than the Unicode data set.  We'll likely require at least data for normalization and collation.
  - Axel provided some additional background.  Windows has NLS interfaces.  ICU was added to Windows 10 two years ago,
    but application developers still want to target Windows 7 where it isn't available.  Windows 10 now supports about
    3-4 times more locales than Windows 7 due to the inclusion of the CLDR.  Carrying ICU with an application is
    becoming a significant servicing issue.  Time zone data bases were integrated into Windows to address similar
    servicing issues, but have to be updated often and quickly for geopolitical reasons.  CLDR data is unlikely to
    be updated as frequently.
  - Henri asked for more details.  How is ICU updated in Windows 10?  Will older Windows 10 releases get updates?
  - Axel responded that the latest available ICU is distributed in each 6 month release cycle, but that locale data and
    Unicode versions are not otherwise patched.  Exceptions could be made, but would require significant motivation like
    the geopolitical reasons that motivate time zone updates.
  - Henri surmised that older Windows 10 releases won't get support for new Unicode characters, data tables, etc...
  - Axel confirmed.
  - Hubert stated that he had heard that the ICU distributed in Windows 10 does not exactly match any official ICU
    release.
  - Axel confirmed adding that patches are made for geopolticial reasons.
  - JeanHeyd stated that the road to UTF-8 support is going to be a long one.  He plans to take papers to the C and
    POSIX committees proposing to change the default locale to UTF-8 in order to facilitate a similar change for C++.
    The goal is to allow applications to at least be able to communicate without mangling text.  It sounds like
    Microsoft is heading in a good direction, but the C committee may be reluctant to make such a change.
  - Axel agreed that this will be a long journey and encouraged everyone to play with the new UTF-8 functionality,
    report problems, and poke application providers to improve support.  The problems are not massive, but cost/benefit
    analysis must line up as always.  Application providers will be required to support UTF-8 as ACP for some Microsoft
    platforms like the Xbox and Hololens.
  - JeanHeyd commented that our biggest concern has been how to migrate to a UTF-8 world.  At least it sounds like there
    is a path to follow.


# May 15th, 2019

## Draft agenda:
- Continue discussion of UTF-8 as execution encoding.  Our focus last time was on impediments to use of UTF-8 as
  execution encoding.  Focus this time will be on anticipated benefits of mandating UTF-8 and impact to existing
  ecosystems.

## Meeting summary:
- Attendees:
  - Cameron Gunnin
  - Henri Sivonen
  - Hubert Tong
  - JeanHeyd Meneide
  - JF Bastien
  - Michael Spencer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Discussion of potential benefits/costs of mandating UTF-8 as execution encoding:
  - Tom introduced the topic with a brief summary from the last meeting.
  - Zach stated he was all for moving to UTF-8.
  - Tom asked how code would be written differently compared to today.
  - Zach presented some problematic examples he has encountered in the past.  The first was surprises with UTF-8
    encoded literals in source code not retaining UTF-8 encoding at run-time.  The second was difficulties writing
    text and having it display in terminals as expected.  Today's compilers don't have options to state that the
    output of a program will have any particular encoding.  Writing non-ASCII output is expert territory.  We can't
    write portable code that dumps text and expect it to just work.
  - Tom noted some subtleties with those examples; there are actually four encodings involved.  Source encoding,
    presumed execution encoding, run-time execution encoding, and terminal/console encoding.  This raises the question
    of what is meant by mandating UTF-8.  Mandating it for all of these encodings?
  - Steve stated that these issues affect all platforms.  Use of characters outside the basic source character set
    doesn't work unless `std::setlocale()` is called to set a locale other than `"C"`.  Changing the default locale
    to `"C.UTF-8"` would be an improvement as it would suffice to make the multibyte conversion functions work as
    expected without changing behavior for character classification functions.  This matches what Python is doing
    as described in [PEP-538](https://www.python.org/dev/peps/pep-0538) and
    [PEP-540](https://www.python.org/dev/peps/pep-0540).  This still allows the locale to be run-time selectable,
    but provides a better default for character encoding.
  - Tom commented that this would preserve existing behavior since, today, unless `std::setlocale()` is called to change
    the current locale, characters outside the basic source character set elicit undefined behavior for the multibyte
    conversion functions.
  - Zach asked how this would be proposed.  By defining a "C.UTF-8" locale?  Or by specifying that the default "C"
    locale operate as if `LC_CTYPE` were set to UTF-8?
  - Steve responded that the implementation behave as though an implicit call to `std::setlocale(LC_CTYPE, "C.UTF-8")`
    occurred during process startup.
  - Henri observed that the behavior of `nl_langinfo` would be affected by doing so; `nl_langinfo(CODESET)` would now
    return a string reflecing UTF-8 rather than the encoding used to implement the "C" locale.
  - Zach noted that such a change needs discussion with WG14 and POSIX members.  It would not be good if behavior
    differed based on whether C or C++ headers were included.
  - JeanHeyd indicated that he is already working on such a discussion and plans to submit a paper to WG14 proposing
    that "C.UTF-8" be made a standardized locale.
  - Steve noted that POSIX exposes more encoding aware facilities than C does; more character classification functions
    for example.
  - Zach asserted that, if we made UTF-8 the default, life would be easier for everyone.
  - Tom summarized discussion so far; we've been discussing changing the default locale, but not mandating UTF-8.
  - Hubert noted that mandating UTF-8 will affect presumed encoding.
  - Henri suggested that mandating a particular encoding might solicit reluctance from implementors.  If implementors
    don't go along, then the standard doesn't match reality.
  - JeanHeyd agreed with the concern and noted that changing the default leaves open an escape hatch for preserving
    existing behavior.  While mandating a particular encoding would make some things easier, it would also leave some
    platforms and/or implementations behind.
  - Tom asked Hubert for his perceptions regarding changing the default; how would the platforms he supports be impacted?
  - Hubert replied that, on z/OS, it would be kind of odd due to the possibility of multiple processes sharing the same
    language environment.
  - Tom asked what happens today if a single process changes its locale.
  - Hubert responded with some uncertainty.  Switching between EBCDIC code pages is much less impactful than switching
    from an EBCDIC code page to something ASCII based would be.
  - Hubert returned to questions of implementation; how would the implicit call to `std::setlocale()` be implemented?
    This isn't a typical language level thing.
  - Tom responded that such a question is what he would have posed to Hubert.
  - Hubert elaborated on potential complexities.  How would this work when separately compiled components potentially
    compiled for different standard versions are linked together?  Is this a linker option?
  - Tom stated that is outside the scope of the standard.
  - Hubert agreed, but noted that concerns like this don't come up very often.
  - Zach reiterated the intent; that the C++ startup code perform as if an implicit call to `std::setlocale()` took
    place.
  - Hubert acknowledged the intent, but noted the potential unintended effects that Henri eluded to earlier.  For example,
    on AIX, Hubert tried a program that displays the current locale encoding.  When invoked with `LANG=C`, it indicated
    ISO-8859-1.  An implicit call to `std::setlocale()` would change this behavior.
  - Tom said he would like to see a concrete example of that behavior.
  - \[Editor's note: Tom later experimented on a Linux system and observed the same behavior:

        $ cat t.cpp 
        #include <langinfo.h>
        #include <locale>
        #include <cstdio>
        
        int main() {
          std::printf("%s\n", nl_langinfo(CODESET));
          std::setlocale(LC_CTYPE, "");
          std::printf("%s\n", nl_langinfo(CODESET));
        }
        
        $ g++ t.cpp -o t
        
        $ LANG=C.UTF-8 ./t
        ANSI_X3.4-1968
        UTF-8

    \]
  - Zach suggested that the multibyte conversion functions don't really work now,
    so changes can be made.
  - Hubert disagreed and stated they work exactly as intended.
  - Steve stated that functions like `std::wctomb` will silently drop or replace characters for the "C" locale.
  - Tom asked Zach if this is an example of what he meant when he said they were broken.
  - Zach replied, yes.
  - Henri noted that silently dropping/replacing characters can be a security issue.  By specifying behavior, we
    may reduce security problems.
  - Hubert agreed that, if you don't do proper error checking, that can lead to problems.
  - Tom asked if these functions have appropriate error handling interfaces.
  - JeanHeyd stated that they do, they return -1 on error.
  - Tom clarified, yes, they return -1 if an ill-formed code unit sequence is encountered, but do they also return in
    error if a character lacks representation in the current locale and a replacement character is substituted?  Tom
    thought at least some implementations substituted replacement characters without errors.
  - Tom summarized the ramifications of an implicit `std::setlocale()` call; doing so is a breaking change given
    Henri and Hubert's observations about querying the current locale encoding.
  - Hubert agreed, noting that non-exotic platforms are impacted.
  - Steve suggested that the observeable impact should be limited to querying locale properties.
  - Hubert stated that may be true, but that we haven't discussed presumed execution ncoding yet.  The standard only
    discusses one execution encoding, but there are two.
  - Tom acknowledged and added that we know this to be problematic as it imposes a lowest common denominator approach
    for encoding literals.  If the presumed execution encoding were UTF-8, that would clearly be problematic on z/OS,
    but even on ASCII based platforms, if the locale can be changed at run-time, that limits use of UTF-8 in literals.
  - Hubert commented that the fallout for z/OS for changing presumed execution encoding may not so bad because
    extensions are already available to specify execution encoding and to opt into an ASCII based encoding.  Additional
    support might be needed for locale encodings.
  - Tom asked to clarify what was meant by additional support.
  - Hubert responded that he was thinking about message catalogs used with translation.  Generally, messages can be
    written in a common subset of characters available in all locales.  If we mandated an encoding that doesn't share
    a common subset (e.g., UTF-8 on a predominantly EBCDIC based platform), then strings would have to be maintained
    in resource files outside of translation units, or written in escape sequences.
  - Tom asked if Unicode escape sequences are used much on z/OS.
  - Hubert responded that, in newer code, yes, but generally only in Unicode literals, not in ordinary or wide literals.
  - Tom returned to the question of benefits of mandating a single execution encoding.  If we did so, then we could make
    text processing decisions at compile-time.
  - JeanHeyd suggested that we can do that anyway.
  - Tom responded that, no, evaluation of functions that are locale dependent must be deferred until run-time since the
    encoding is not known.
  - Tom asked about the feasibility of changing the default locale given existing ecosystems.  We would have to work with
    POSIX, WG14, implementors, other languages?
  - Zach reiterated that he wants to see the portability problems writing to terminals be fixed.
  - Tom asked, but isn't that a problem not specific to C++?  All languages face this issue.
  - Zach noted, for all processes, data comes out as bytes, but terminals can't handle them.
  - JeanHeyd noted that codecvt facets may interfere, but the primary difficulty programmers face on Windows is that the
    default terminal settings and execution encoding are locale dependent.
  - Zach observed that, if everything was UTF-8, then the terminal could just do the right thing.  But we can't mandate
    behavior for all languages.
  - Henri asked if Microsoft's new terminal might help.
  - Tom stated that it should, but will require some form of opt-in.  Historically, Microsoft's default console encoding
    has been constrained by backward compatibility.  The encoding of the console differs from the locale encoding in
    order to support those old applications that depend on line drawing character sets.  Unclear how new applications will
    opt-in to UTF-8 behavior.
  - Zach suggested that just having a way to determine if the current locale supported UTF-8 would be useful.
  - Henri asserted that the experience with Python demonstrated that such approaches don't work in practice.  Asking the
    C environment what the execution encoding is rather than just assuming it may actually cause more problems.  What
    would you do if the current encoding wasn't compatible?
  - Zach replied that, if the encoding is known, then the program can convert.
  - Tom stated that is how the multibyte conversion functions are already specified to work; if they are used, then the
    output will match locale encoding.
  - JeanHeyd expressed having unsatisfactory experience with the multibyte conversion functions.  For example, Microsoft's
    implementation of `mbrtoc32` unconditionally converts between UTF-8 and UTF-32 in violation of the standard.  The
    conversion functions are also unable to detect some kinds of mojibake; for many single byte encodings, all possible
    code unit sequences are valid.
  - Hubert concurred that the multibyte conversion functions are not a good alternative to ICU.  Discussions about
    `source_location()` are touching on a bigger problem; C++ is just one language residing in a large ecosystem in which
    implementors are beholden to the platforms they support.
  - Henri asked about wide characters being compatible across locales.  What encoding does z/OS use for them?
  - Hubert deferred to documentation and noted that, on AIX, the wide character set does vary for some Chinese locales.
  - Tom stated that the wide character set on z/OS is a wide form of EBCDIC with support for ISO-2022 escape sequences.
  - Hubert followed up regarding the wide character sets used on AIX.  Depending on the size of `wchar_t`, either
    UTF-16 or UTF-32 is used except for the Taiwanese locale which uses Big-5.
  - Tom asked JF what benefits Apple might see from changing the default locale encoding to UTF-8.
  - JF responded that there would be some simplicity.
  - Tom pondered if the biggest benefit to Apple would come from dropping locale dependent encoding completely.
  - Hubert asserted that isn't feasible; IBM customers are known to create their own custom locales.
  - Steve noted that one way in which applications are meeting the requirement for GB18030 by the PRC is via locales.
  - Henri expressed a belief that the PRC doesn't actually require GB18030 support; rather that the mandatory character
    subset from it be supported.
  - Steve stated that he was required to add support for GB18030 and did so via conversions at program boundaries.
  - JeanHeyd suggested that no one is really looking to deprecate locale support.  We just want to make text processing
    easier and better locale facilities would help.
  - Henri stated that he would like to deprecate broken character classification functions like `isalpha()` since they
    can't properly handle Unicode inputs.
  - JeanHeyd suggested that, if we had new and improved locale primitives, then we could deprecate the old ones.
- Discussion then moved on to an issue that JF raised on the mailing list concerning Unicode characters in identifiers.
  - http://www.open-std.org/pipermail/unicode/2019-May/000367.html
  - https://github.com/sg16-unicode/sg16/issues/48
  - Tom briefly introduced the topic.  The C++ standard specifies a subset of Unicode characters that may be used in
    identifiers.  That specification is via a series of code point ranges in
    [[lex.name]p1](http://eel.is/c++draft/lex.name#1), but the standard doesn't offer a rationale for the specified
    ranges.  It seems likely that the ranges aren't being maintained as Unicode evolves.
  - JF stated that the current ranges lack justification, but that isn't much of a problem in practice.  Clang accepts
    identifiers using the specified code points, but gcc does not, so such identifiers are not portable in practice.
    Perhaps the standard should just adopt and defer to [UAX#31](https://unicode.org/reports/tr31).
  - Henri asked what the motivation is to address this if it isn't a problem in practice.
  - JF responded that it is a problem in principle.  It is unclear why gcc allows the code points in identifiers if
    specified via `\u` escapes, but not when the actual characters are present in the source encoding.  If the standard
    was more precise, perhaps implementations would be more consistent.  Also, such characters should be allowed in
    module names and we should use the same identifier scheme there.
  - Henri asked how much of the concern is separating identifiers from operators.
  - JF responded little since there are no plans to, for example, adopt Unicode math symbols as operators.
  - Hubert provided a little history.  The existing code point ranges were provided by Clark Nelson in WG14's
    [N1518](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1518.htm) and were based on the version of
    [UAX#31](https://unicode.org/reports/tr31) current at that time.
  - Hubert stated that it would be annoying to have the standard defer to a Unicode TR as doing so makes it that much
    more difficult to determine what the actual rules are for a given standard.
  - Tom suggested that deferring to a Unicode TR would at least have the benefit of the ranges matching whatever version
    of the Unicode standard the implementation adheres to.  This would presumably help maintain the ranges as characters
    are added over time.


# April 24th, 2019

## Draft agenda:
- Discuss execution encoding, locale dependency, impediments to supporting UTF-8, and the feasibility of mandating UTF-8.

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - Henri Sivonen
  - JeanHeyd Meneide
  - Mark Zeren
  - Nathan Myers
  - Steve Downey
  - Tom Honermann
- Discuss execution encoding, current locale dependency, and the feasibility of mandating UTF-8.
  - Tom initiated discussion by asking for a list of issues in the standard that don't work as expected when the
    execution encoding is UTF-8.
  - Steve led with the observation that locale settings default to the "C" locale.  A call to `std::setlocale` is
    required to enable characters outside the basic source character set.  Failure to set the locale results in
    functions like `mbrtoc16`, `c16tombr`, and `mbrlen` failing when such characters are present.
  - Steve added that iostreams requires I/O to match the current locale in order to meet expectations of attached
    `std::codecvt` facets.
  - Tom asked Nathan if he knew the history of where `std::codecvt` originated.
  - Nathan responded that it was in the original design.  The intent was for it to perform conversions for external
    I/O for the benefit of other parsing functions like for parsing numbers.  The design was intended less for
    `std::cin` and `std::cout`, but more for bespoke streams.
  - Steve mentioned that JeanHeyd had recently discussed another issue.
  - JeanHeyd explained the problem with requirements that `std::basic_filebuf` place on `std::codecvt` facets in
    [[locale.codecvt.virtuals]p3](http://eel.is/c++draft/locale.codecvt#virtuals-3); that the mapping of characters
    is 1 internal character (code unit) to N external characters (code units).  This requirement prohibits support
    for variable length encodings when the internal character type is not large enough to hold a decoded code point,
    as is the case with any `std::codecvt<char, char, ...>` specialization that would be used to convert between,
    for example, UTF-8 and ISO-8859-1.
  - Tom asked if this requirement causes any problems when the `std::codecvt` facet is a `noconv` one.
  - JeanHeyd clarified that the requirement prohibits arbitrary chunking of input/output because there is no
    provisions for decoding/encoding a partial code unit sequence.
  - Henri asked if `std::basic_filebuf` is broken for every multibyte encoding.
  - JeanHeyd responded yes, according to standard implementors he has discussed this with, broken and unfixable.
  - Tom wondered why this limitation exists as it sems to imply a buffer of size 1.
  - Nathan commented that, since it is already broken, we can't break it further.
  - Tom disagreed noting that it works for single byte encodings.
  - Tom surmised that lifting the restriction might require an ABI break and wondered how much use of
    `std::codecvt` exists in the wild and would be affected.
  - Nathan asked if this issue only affects files.
  - Tom responded that the limitation is present only for `std::basic_filebuf` and not for `std::basic_streambuf`,
    so presumably yes, only affects files.
  - Nathan provided some historical context; when iostreams was introduced, wide characters were expected to be
    1x1 mappings from code unit to code point, so variable length encodings would be unnecessary.  Most UNIX
    systems adopted a 32-bit `wchar_t`.
  - Nathan suggested that, given the issues with it, we don't need to worry about improving `std::codecvt`.
  - Steve responded that its presence gets in the way of implementing something else.
  - Henri asked if it would be feasible for C++ startup to implicitly initialize the locale to UTF-8.
  - Steve noted that doing so would break existing code.
  - Nathan stated that we don't need to actually change the C or POSIX locale, we can define our own.
  - Tom asserted that would be surprising to programmers expecting facilities to continue working as they do today.
  - Steve added that programmers expect `printf` and `std::cout` to work together.
  - Nathan responded that writes via `printf` go into a buffer and that writes to `std::cout` go to the same buffer.
  - Steve countered that `std::codecvt` facets only hook iostreams.
  - \[Editor's note: A few minutes of discussion were missed here \]
  - JeanHeyd mentioned that the "C" locale doesn't have to match the system encoding.
  - Tom stated that it has to be compatible though.
  - Steve added that it must also be a single byte encoding.
  - \[Editor's note: Another few minutes of discussion were missed here \]
  - Tom stated that there needs to be a protocol for agreeing on encoding at both ends of a stream.  That protocol
    is currently the current locale setting.  Writing something other than what a terminal expects is mojibake.
  - Steve noted that much existing code works because streams are typically just bytes that are passed through.
  - Tom added that most programs only deal with ASCII and break with non-ASCII characters.
  - Corentin expressed two view points regarding status quo:
    - 1) The current locale setting is a guess.
    - 2) There is a conversion loss if the current locale cannot represent all characters from the internal encoding.
  - Tom disagreed that current locale is a guess; implementations are required to know locale settings.
  - Nathan claimed that the standard can lead implementations; that we can provide a target for implementors to aim for.
  - JeanHeyd disagreed with that notion; if we make execution encoding UTF-8, implementors can aim for it over time.
    However, it would be better to abandon execution encoding and focus on bytes.  Changing execution encoding breaks
    existing machinery, changes the encoding of literals, causes `codecvt` facets to observe different data, etc...
    Making execution encoding UTF-8 is not a practical reality; it is too big a breaking change.
  - Henri offered another perspective.  glibc still offers traditional locales, but back around 2002, RedHat decided
    that GTK would use UTF-8 internally.  To support existing file names, GTK provided `G_FILENAME_ENCODING` and
    `G_BROKEN_FILENAMES` environment variables that influenced interpretation of file names.  Debian followed suit
    in 2007.  The Linux ecosystem has moved to UTF-8.  Now that Microsoft has support for UTF-8 on terminals and is
    UTF-16 internally, Microsoft should be able to migrate.
  - Tom acknowledged that that matches his understanding of what Microsoft is working towards.
  - Steve observed that most programmers don't want locale to affect their streamed data, they just want to write bytes.
  - Henri asked about the feasibility of the implementation transcoding implicitly.
  - JeanHeyd responded that `codecvt` gets in the way.
  - Tom added that filenames are a problem as, in general, they can't be transcoded without harming them.
  - Henri provided some background on how Rust handles file names.  A separate type is used for file names.  On
    non-Windows platforms, file names are sequences of bytes and are decoded as text for presentation, but may not
    roundtrip.  On Windows, WTF-8 is used.
  - Steve noted that `std::filesystem` could operate similarly.
  - Henri added some additional Firefox history.  At some point, a bug was introduced that resulted in it only
    supporting UTF-8 file names.  When the problem was discovered, a decision was made to only support UTF-8 file
    names and that remains the current policy.
  - Nathan stated that he is not worried about breaking things because implementors will retain support for prior
    behavior.
  - JeanHeyd countered that some implementors may take an over-my-dead-body stance on mandating UTF-8.  We don't want
    to introduce yet another language dialect.


# April 10th, 2019

## Draft agenda:
- Continue discussion of JeanHeyd's D1629R0: Standard text encoding
- Discuss any further work-in-progress from Corentin and Martinho on code point properties, and normalization.
- Discuss execution encoding, current locale dependency, and the feasibility of mandating UTF-8.

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - Hana Dusíková
  - JeanHeyd Meneide
  - Mark Zeren
  - Steve Downey
  - Tom Honermann
- Continue discussion of JeanHeyd's D1629R0: Standard text encoding
  - JeanHeyd walked us through the code he has been developing to prototype interfaces to be proposed.
    - https://github.com/ThePhD/phd/tree/master/include/phd/text
    - Encoding types have `encode()` and `decode()` member functions that accept an input range, an
      output range, a state, and an error handler and return an `encoding_result` type that forwards
      the possibly mutated input range, output range, and state.  These functions operate on a single
      code point at a time.
    - `text_transcode` and `text_transcode_into` interfaces are provided for conversion of multiple
      code points at a time.  Generic implementations are provided, but the design is intended to
      support optimizing for contiguous ranges or characteristics of specific encodings.
  - Tom asked if input and output iterators/ranges are supported or if forward iterators/ranges are
    required.
  - JeanHeyd replied that input and output iterators are supported, but an error handler won't be able
    to observe the code units that provoked invocation of the error handler.
  - Tom suggested that a
    [caching iterator](https://github.com/tahonermann/text_view/blob/master/include/text_view_detail/caching_iterator.hpp)
    can be used to solve that problem.  This is the approach used by
    [text_view](https://github.com/tahonermann/text_view).
  - Steve added that the state type can also be used to cache such code units.
  - JeanHeyd explained more about the error handling.  `encoding_result` can store an error status
    and any additional useful information.  The implementation uses the facilities provided by the
    `<system_error>` header.
  - Tom asked for clarification; ranges are always moved into and back out of error handlers?
  - JeanHeyd answered, yes, via `encoding_result`.
  - Tom asked about the possibility to encode only a state change without encoding a character.
  - Steve asked for clarification; as in for a ISO-2022 style shift sequence?
  - Tom confirmed.
  - JeanHeyd replied that the state type can be used for whatever purposes.
  - Tom expressed skepticism about that working from an interface perspective since both input and
    state are provided.  Sometimes, you just want to encode a state transition.
  - Steve noted that state is strongly tied to encoding and asked if state needs to be exposed in the
    interface.  The ability to resume a conversion is still necessary, but wouldn't have to be handled
    via state.
  - JeanHeyd stated that having the state be separate is useful for flexibility in resumption.
  - Steve added that state often becomes a house keeping burden.  Users generally don't know how to
    work with it and do things like passing an initial default constructed state when a resumption
    state is needed.
  - Tom asked how much JeanHeyd had reviewed
    [text_view](https://github.com/tahonermann/text_view) as some of what is being discussed appears
    to be reinventing solutions implemented there.
  - JeanHeyd responded that the interfaces were influenced by reviews of
    [Boost.text](https://github.com/tzlaine/text),
    [text_view](https://github.com/tahonermann/text_view), and
    [Ogonek](https://github.com/rmartinho/ogonek).
  - Corentin asked if the transcoding interfaces can provide lazy ranges.
  - JeanHeyd responded no, not yet.
  - Steve stated that lazy ranges don't necessarily play well with optimized transcoding operations.
  - JeanHeyd stated that the interface is intended to allow optimization.  Implementors can use
    `if constexpr` internally.
  - Tom expressed concern about reliance on `if constexpr` within an encoding agnostic generic
    function and suggested specialization as a more extensible solution.
  - JeanHeyd explained that overloading can be used to provide more optimized implementations
    without lots of specializations.
  - Tom observed that the trade off is a bunch of overloads vs a bunch of specializations.
  - JeanHeyd acknowledged the trade off, but noted that often fewer overloads are required because
    conversions can be relied on.
  - Tom suggested that, perhaps, there should be a `std::transcode` customization point.
  - JeanHeyd acknowledged and said he is still playing around with such ideas and wants to enable
    users to provide custom overloads.
  - Tom expressed hope that users won't be writing these often at all; that such interfaces should
    mostly be written by library providers.
  - Steve agreed and added, or small infrastructure teams.
  - Steve asked about type erasure and support for dynamic encodings.
  - JeanHeyd expressed uncertainty about the need to provide that within the standard and illustrated
    how a custom encoding that handles dynamic encodings could be written.
  - Steve added that POSIX provides iconv which allows requesting a codec for a named encoding and
    such functionality should be provided.
  - Tom suggested it might suffice to be able to write an `iconv_encoding` type that wraps iconv.
  - Tom asked how transcoding between encodings with different associated character sets are handled.
  - JeanHeyd responded that no support is present yet, but that attempting to transcode between such
    encodings would fail compilation it the code point types were not compatible.
  - Tom observed that failing compilation requires a strong type, not `char32_t`, to enforce safety
    via the type system.
  - Corentin asserted that `basic_text_view` should not have a template parameter for normalization
    since normalization is not relevant for all encodings.
  - Tom agreed that, if present at all, normalization should be incorporated into the encoding type.


# March 27th, 2019

## Draft agenda:
- Discussion with JF Bastien regarding JavaScript support for Unicode regular expressions.
  Based on outcome, JF can arrange for JavaScript maintainers to attend a future meeting.
- Discuss any work-in-progress from JeanHeyd, Corentin, and Martinho on transcoding, code point
  properties, and normalization.
- Discuss the recent LEWG mailing list emails regarding iostreams and char8_t support.

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - Hana Dusíková
  - Hubert Tong
  - JeanHeyd Meneide
  - JF Bastien
  - Mark Zeren
  - Michael Spencer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Discussion with JF Bastien regarding coordinating Unicode regular expression support with ECMA TC39
  and the ECMAScript standard.
  - JF introduces:
    - In contrast to what was stated in Kona, ECMAScript does specify Unicode support for regular expressions.
    - It would be beneficial to align C++ and ECMAScript support for Unicode regular expressions.
    - ECMA TC39 is currently working on adding support for Unicode sequence properties.
    - ECMA TC39 is coordinating their work with the Unicode Consortium.
    - SG16 should get involved and collaborate with ECMA TC39.
    - JF provided the following links for reference purposes:
      - https://github.com/tc39/proposal-regexp-unicode-sequence-properties
      - https://unicode.org/L2/L2018/18337-broaden-properties.pdf
      - https://www.unicode.org/L2/L2019/19056-prop-cmts.pdf
      - http://unicode.org/reports/tr18/
  - JF volunteered to connect interested SG16 participants with ECMA TC39 participants and asked for
    volunteers.
    - Zach expressed interest with the specifc desire for C++ to be aligned with the defacto standard (ECMAScript).
    - Hana opted in with explicit concerns regarding whether the C++ standard can defer to ECMAScript for
      wording specification.
    - Tom expressed interest in following along, but has no significant ECMAScript or Unicode regular expression
      experience to contribute; mostly intersted in staying informed for SG16 administrative purposes.
    - Corentin said, sure, why not.
  - Tom, responding to Hana's concern, stated that, by working with TC39 we can help ensure that the ECMAScript
    standard can be referenced normatively by the C++ standard.
  - Hana noted that, in Kona, we stated a goal of supporting [UTS#18](http://unicode.org/reports/tr18) level 1,
    but ECMAScript doesn't meet that.
  - Tom stated that is a good reason to work with them to find out why it isn't implemented.
  - Hana provided a link discussing why level 1 is not met:
    - https://github.com/tc39/proposal-regexp-unicode-property-escapes
  - Zach suggested that, if ECMAScript doesn't support it, we probably don't need to either.  Small differences
    in what is supported in different languages is annoying because programmers tend to think it is ok to copy
    and paste between languages, but then get different behavior.
  - Zach suggested researching what level of UTS#18 support ICU provides.
  - JF noted most implementations defer to ICU and only inline simple expressions for performance.
  - Corentin observed that full UTS#18 support is madness and subsetting is necessary.
  - JF stated it would be beneficial to identify an appropriate subset of UTS#18 for both standards.
  - Hana noted that, in the link she provided, TC39 states what is required and discourages implementors from
    offering extensions in order to preserve portability and compatibility.
- D1628R0: Unicode character properties
  - Draft document available in the SG16 mailing list archives:
    - http://www.open-std.org/pipermail/unicode/2019-March/000266.html
  - Corentin presents:
    - Goal: Provide useful properties from [UAX#44](http://www.unicode.org/reports/tr44).
    - Goal: Enable querying properties for any code point for a subset of the UAX#44 properties that are deemed
      generally useful.
    - A reference implementation is available, but is considered early work:
      - https://github.com/cor3ntin/ext-unicode-db.
    - Hana has been using the reference implementation in CTRE to provide Unicode regular expression support
      in constexpr form.
    - The interface is specified as a set of predicate functions and enumerations.
    - How to handle Unicode versions is an open question.  Some properties tend to be stable (e.g., general
      category), others are less so (e.g., script, directional, joining).
    - Corentin compared the last 5 Unicode standards for differences in property values and found little change.
    - Multiple version support is needed in order to provide a stable and portable interface while allowing for
      implementors to provide newer versions.
  - Michael asked about providing different versions of the algorithms as doing so could require table duplication.
  - Zach reported discovering that requirement as well.  Multiple versions of the algorithms can't be provided
    without duplicating some tabular data.  This could double the necessary storage foot print.
  - Zach observed that multiple versions of code point properties isn't useful if version specific algorithms are
    not provided.
  - Zach stated he didn't see a reason to expose the "age" property.
  - Corentin offered two use cases for properties:
    - For use in implementing the Unicode algorithms.
    - For use by general programmers for non-algorithm purposes.
  - Tom stated that it seems problematic to potentially have user code using a version other than what the
    standard library is using.
  - Zach noted that providing multiple versions goes against our existing guidance allowing implementors to
    float the Unicode version.
  - Steve observed that the specified interfaces are constexpr, but ICU doesn't provide data in constexpr form.
  - Tom responded that the interfaces could be implemented using intrinsics that defer to ICU or a custom database.
  - Corentin added that most tables are small with the name table being a notable exception.  The constexpr
    design allows linking only what is needed.
  - Tom asked, won't you still need the tables for run-time calls?
  - Corentin responded that the tables could be linked in only if referenced.
  - Zach stated that isn't dependable; implementors might have to link them in anyway.
  - Zach listed a few specific concerns with the paper:
    - `codepoint` is marked as exposition only but can't be because it is named in specified interfaces.
    - `codepoint` needs to support `char` and `wchar_t`.
    - Interfaces taking code points should accept any integral type, encoding is not relevant here.
  - Corentin explained that the `codepoint` type was introduced specifically to not allow `char` and
    `wchar_t` in order to avoid calls with character literals that might not be ASCII based.
  - Zach stated a preference for integer values anyway.
  - Tom noted that using a `codepoint` type allows using the type system to catch mistakes.
  - Zach stated that type safety is illusory because `char` and `wchar_t` are code units not code points.
  - Tom countered that accepting `char` and `wchar_t` is useful for character literals, but only for
    character literals.
  - Corentin mentioned that he wants the interface to be noexcept all the way through and that wide
    contracts be used to avoid UB.
  - Michael stated that these should be Unicode scalar values instead of code points then since any
    value is valid.
  - Corentin agreed, the interface is defined for any integer; the predicates just return false if the
    value isn't a valid code point.
  - Steve suggested we may want code point and scalar value types with contracts, but that probably
    depends on alignment with `text_view` and ranges.  Maybe that is only useful at a higher level than
    is needed for code point properties.
  - Zach predicted that LEWG will object to the "cp_" prefix on these interfaces.
  - Zach expressed a desire for more motivation in the paper; to demonstrate a need or justification
    for each exposed property.  Examples of why a regular programmer would care about each of these.
    Maintaining large interfaces or lots of properties complicates teaching.
  - Corentin explained wanting to provide replacements for some broken things in the standard, like
    `std::isalnum`.  Having these available will help programmers use Unicode properly.  As an example,
    they are needed to implement Unicode regular expression support.
  - Zach acknowledged, just want to see that motivation expressed in the paper.
  - Tom agreed with adding explicit motivation like the `std::isalnum` example.  Not necessarily code
    examples, but scenarios.
  - Zach observed that some properties can be used incorrectly because they typically aren't used in
    isolation.  Some properties should only be used via the Unicode algorithms.
  - Hana stated a preference for exposing the Unicode standard as it is specified.  Considerable work
    and expertise has gone into it.  It is a standard that people can learn.
  - Zach disagreed on a philosophical basis; want to keep things simple.
  - Steve observed that some of this is ergonomics of naming.  Programmer should reach for a function
    first, then raw properties only if necessary.
  - Zach cautioned about exposing an expert-only interface.
  - Corentin mentioned, by defering to Unicode, we avoid making mistakes.  Properties that are only
    used for derived properties are already excluded.
  - Tom stated that we can always expose additional properties later as we identify use cases.
  - Michael stated that some properties are implementation detail within the Unicode standard; they exist
    for the algorithms to refer to them.  We should focus first on high level interfaces and those
    probably won't be defined in terms of low level property interfaces.
  - Zach stated that properties that are only needed to implement an algorithm need not be exposed
    individually.
  - Tom asked about tailoring and properties for the private use area (PUA).
  - Corentin replied that tailoring should be provided by a separate interface.  The PUA shouldn't be
    used in open interchange.
  - Tom agreed, but stated that, within an application, a programmer might want all libraries to see the
    same customized properties for the PUA.
  - Steve noticed that the Unicode version numbers in the `version` enumerator values jump from `0x09`
    to `0x10` rather than to `0x0A`.
  - Corentin replied, oops.
  - Corentin added, if we don't support multiple Unicode versions, there is a question of enumeration
    value stability across implementations and differing Unicode versions.
  - Tom suggested that we'd like to see a revision of the paper and asked for objections.
  - No objections were raised.
- D1629R0: Standard text encoding
  - JeanHeyd screen shared an early draft that has not yet been published, so no link is available.
  - JeanHeyd presents:
    - The proposed design follows review of a number of prior papers and projects:
      - [P0244 - Text_view: A C++ concepts and range based character encoding and code point enumeration library](http://wg21.link/p0244)
      - [text_view](https://github.com/tahonermann/text_view)
      - [libogonek](https://github.com/libogonek/ogonek)
      - [Boost.Text](https://github.com/tzlaine/text)
    - Enabling optimizations is a goal.
    - Want a range based approach.  Want to enable lazy encoding/decoding.
    - Wrapping iterators can be large, iterator/sentinel pairs are helpful to reduce iterator sizes.
    - Can't depend on locale or `codecvt` because of performance costs; `wstring_convert` exhibited these costs
      in [Sol2](https://github.com/ThePhD/sol2).
    - Exposing state enables chunked streaming.
    - An empty state can indicate a self-synchronizing encoding.
    - State could be potentially omitted in interfaces for stateless encodings.
    - The default error handler will substitute replacement characters.
    - Error handling can be elided by specifying an assume_valid handler.
    - Sized output ranges can be used for memory safety.


# March 13th, 2019

## Draft agenda:
- Post Kona review and follow up.
- RFP: Transcoding interfaces.
- RFP: Code point and EGC iterators.

## Meeting summary:
- Attendees:
  - Bob Steagall
  - Corentin Jabot
  - JeanHeyd Meneide
  - Martinho Fernandes
  - Michael Spencer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
- Post Kona review and follow up.
  - Tom started discussion with a proposal to initiate a draft working paper for SG16 proposals.  The intent
    being to start drafting wording for features we'd like to get into C++23.
  - Corentin expressed concerns that a large paper could prompt calls for a TS approach and that small focused
    papers would be preferred.
  - Steve noted that a consolidated paper has an advantage in demonstrating how the features work together.
  - Peter observed that we will have co-dependent parts.
  - Michael expressed distaste for large papers with loosely connected features; everything shouldn't go in
    one paper.
  - Tom suggested brainstorming a list of features we'd like in C++23.  The following were suggested:
    - `std::text` and `std::text_view`.
    - Unicode support for regular expressions.
    - Unicode code point properties.
    - Transcoding and transliteration interfaces (both compile-time and run-time).
    - A trie container.
    - A rope container.
    - Unicode algorithms.
    - Normalization.
    - A code point type.
    - A (Unicode) scalar type.
  - JeanHeyd suggested separate papers could address code point properties, a code point type, and a scalar type.
  - Martinho stated that risks getting a result that isn't useful depending on what features do/don't make it in.
  - Tom mentioned that the Design Group has stated a preference for complete and cohesive proposals.
  - JeanHeyd stated that a collection of papers can demonstrate a cohesive design.
  - Steve observed that large papers are hard to review and provided the Ranges and Networking proposals as examples.
  - JeanHeyd suggested starting with determining what we need for the 5 standard mandated encodings without support
    for extensions.
  - Steve noted that support for extensions may demand different interfaces for lossless vs lossy conversions and
    different error handling.
  - JeanHeyd countered that the ordinary and wide encodings already may be lossy, so such support is already needed.
  - JeanHeyd suggested that strong typing can be used to provide interfaces that SFINAE based on requirements; e.g.,
    that can require non-lossy code point conversions.
  - Tom brought discussion back to the working paper idea and noted that such a paper can help drive a consensus
    based process.
  - Steve expressed another concern with small papers; we can end up with features being copied or re-implemented
    in different papers.  For example, `source_location` in both the Library Fundamentals TS and in Contracts.
  - Corentin asked what the dependencies are between each of the brainstormed features.
  - Martinho stated that there is a core set that needs to be done first.  Discussion identified the following:
    - Decoding and Encoding of UTF, both compile-time and run-time.
    - Unicode properties with tailoring for the private use area.
    - Normalization.
  - Corentin asked if we could postpone tailoring and dependencies on localization.
  - Martinho expressed a preference for Swift's model; default interfaces use the default locale, tailored
    interfaces support providing a locale.
  - Peter stated that, if tailoring is postponed, there is some risk of defining interfaces that won't work with
    tailoring.
  - Corentin suggested that, if we need locale support, we need to start from scratch.
  - JeanHeyd countered that, what Unicode demands from the locale is different than what is provided by
    `std::locale`.
  - Tom asked, for locale support, do we rely on OS settings?  Or require the program to establish a locale?
  - Steve noted that, today, by default, we get the "C" locale unless `std::setlocale` is called.
  - Tom drew discussion back to the working paper idea and asked for volunteers to work on core features.
    - JeanHeyd volunteered to work on transcoding and transliteration features.
    - Corentin volunteered to work on code point properties.
    - Martinho volunteered to work on normalization.
- Discussion of D1515R0 - Code points, scalar values, UTF-8, and WTF-8
  - https://rmartinho.github.io/cxx-papers/d1515r0.html
  - Martinho presents.
    - There is a trade off.  If we favor code points, then interfaces can work with WTF-8, Windows file names,
      and other bad data.  If we favor scalar values, then we don't have to check for the presence of surrogate
      code points.
  - Steve asserted that we need to be able to work with ill-formed UTF text, but only at program boundaries.
  - Tom suggested there are reasons to support both code point types and scalar value types.
  - JeanHeyd agreed that both are needed, but that scalar values should be preferred.
  - Bob asked if data can be appropriately round-tripped if conversions are done at program boundaries.
  - Tom replied that a higher level protocol is needed for that.
  - Steve provided the example of CDATA in XML.
  - Martinho reported seeing use of the private use area to preserve invalid values.
  - JeanHeyd asserted that handling of invalid values should require some form of opt-in.
  - Peter responded that the opt-in is the choice of encoding and transcoding or transliteration operations.
  - Corentin asked about the motivation for both a code point type and a scalar type.  Why not just rely on
    UB which is what contracts would provide anyway?
  - JeanHeyd asked rhetorically where the contract would be written if there wasn't a type.
  - Tom elaborated, if the scalar type is a class type, then the contract goes on constructors and assignment
    operators.
  - JeanHeyd asked about how to handle WTF-8 via an encoding.
  - Peter responded that you define a transcoder from WTF-8 to UTF-8.
  - Peter also suggested that, instead of using the private use area to preserve invalid values, you map them to
    something else, some kind of substitution character or marker.  This doesn't necessarily preserve roundtripping,
    but allows handling.
  - Steve noted that, if transcoding/transliteration interfaces are extensible, then we don't have to solve this
    problem.
- https://github.com/cplusplus/draft/pull/2768
  - Tom introduced the issue; this PR to the C++ standard was marked as requiring SG16 review.  The issue was
    created by Richard following his editorial review of the wording changes in [P1139R2](http://wg21.link/p1139r2).
  - Tom asked that everyone take a look and offer any feedback.
- Tom announced plans for our next meeting.  It will be on 3/27 and JF has volunteered to arrange for us to talk
  with the JavaScript team about their support for Unicode regular expressions.  At this meeting, we'll discuss
  what we might want to ask/learn from them.  If warranted, JF will then seek to arrange a future meeting.


# February 13th, 2019

## Draft agenda:
- Preparation for Kona.
- Discuss P1228R1 - A proposal to add an efficient string concatenation routine to the Standard Library (Revision 1)
  - https://wg21.link/p1228r1
- Discuss P1439R0 - Charset Transcoding, Transformation, and Transliteration 
  - https://wg21.link/p1439r0

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - Hubert Tong
  - JeanHeyd Meneide
  - Jorg Brown
  - Mark Zeren
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Victor Zverovich
  - Zach Laine
- Preparation for Kona.
  - Tom mentioned that we meet after EWGI and LEWGI have wrapped up for the week.
  - Steve observed that we can become a roadblock for proposals if we meet late in the week.  That probably 
    isn't a concern for this meeting, but could be for future meetings.
  - Peter noted that the `char8_t` remediation paper needs scheduling in LEWG.
  - Tom stated he will reach out to Titus.
    \[Editor's note: Tom checked the LEWG schedule and [P1423R0](http://wg21.link/p1423r0) is on the P1 priority
    list to be slotted in ad-hoc.  Titus expects to get through all P1 priority papers\]
  - Corentin asked about scheduling for [P1097R2](http://wg21.link/p1097r2), Martinho's named character escapes proposal.
  - Tom responded that he doesn't think of it as targeting C++20 due to lack of implementation experience.
  - Zach stated it shouldn't be hard to implement.
  - Tom asked if escape sequences impact the preprocessor.  Would preprocessors require updates?
  - Hubert responded that they shouldn't, though `_Pragma` is potentially impacted due to sometimes needing
    to reverse translation of the literal.
  - Corentin observed that the paper is already on EWG's schedule for Saturday.
- [P1228R1: A proposal to add an efficient string concatenation routine to the Standard Library (Revision 1)](http://wg21.link/p1228r1)
  - Jorg introduced the paper:
    - The proposed design has been in use at Google for 12 years and is available in Abseil as `StrCat`.
    - The design is motivated by performance and desire for a simple API.  Only two overloads are proposed.
    - The design has been discussed on the LEWG mailing list.
      - http://lists.isocpp.org/lib-ext/2019/01/9692.php
      - http://lists.isocpp.org/lib-ext/2019/01/10020.php
    - The design does not have Unicode dependencies.
  - Corentin observed that there is overlap with `std::format`.  Can internals be shared?  Are customization
    points duplicated?
  - Zach stated that seems like more of a discussion for LEWG to have.
  - Corentin stated that the interface should prevent mixing types with potentially different encodings.
    E.g., `std::string` and `std::u8string`.
  - Victor agreed that it should not be possible to mix differently encoded strings.
  - Zach suggested that it is reasonable to only support `char` initially.  Once you step outside of `char`,
    other locale considerations kick in.
  - Tom disagreed with locales only being a concern outside of `char` and professed agreement with the proposal
    that locales be a separate concern.
  - Hubert agreed with the proposal scope; avoid conversion aspects for both encoding and formatting, don't invite
    the complexities exhibited by stream inserters.
  - Jorg stated that having to consider locale would kill performance.
  - Tom asked if anyone wanted to argue for locale awareness and got no responses.
  - Corentin asked about dropping support for integral and float types such that only string types would be supported.
  - Hubert agreed with that direction on the basis that, with numeric types, you often want locale support.
  - Zach agreed observing that the proposed functionality seems to be conflating concatenation and formatting in a
    single API.
  - Hubert observed that it is useful to be able to request how much space would be needed for a numeric conversion.
  - Jorg stated that basic (non-locale aware) integer formatting is cheap (4 instructions on Intel), but not for
    floating point.
  - Hubert mentioned that it is also useful to be able to query a maximum buffer size for a type.
  - Jorg suggested that `std::numeric_limits` supports that.
  - Hubert disagreed; It says how many digits roundtrip, not how many might be printed.
  - Corentin observed that concatentation of differently normalized strings can change the number of perceived
    characters.
  - Peter asked why that is a problem.
  - Zach responded that combining NFC can result in fewer extended grapheme clusters than the two strings by
    themselves contained.
  - Tom expressed distaste for section III.A and treating `char` as an integral type instead of a character type.
  - Peter agreed, characters should be characters.
  - Jorg explained the direction was taken to handle `int8_t` and friends that are often defined in terms of `char`.
  - Zach suggested letting deduction work, `'0'+1` deduces as `int`.
  - Peter asked about section III.A and whether it is always customary for a minus sign to precede a negative number.
    In financial contexts, it sometimes follows the number.
  - Zach noted that the minus sign may appear at the end in RTL languages.
  - Tom asked for additional argumentation regarding treating `char` as an integral type vs a character type.
  - Zach suggested following the precedent set by `std:string::operator+()`; accept the kinds of arguments that it
    does and handle them the same way.
  - Jorg stated that, without numeric conversions, users would have to call `to_string` which is more expensive.
  - Hubert suggested the use of a proxy type when conversion is intended.
  - Peter posted a link to example code he had previously written that used a rope to build a string.
    - https://github.com/dascandy/s2/blob/master/tests/string/test_simple.cpp
  - Tom summarized, the dea is to use `operator+` to construct a rope that is evaluated and collapsed upon
    assignment to a `std::string` or other concrete type.
  - Hubert noted that such approaches have difficulty with `auto`.
  - Peter acknowledged that the result is `rope` if no conversion is specified.
  - Zach asserted this is not a problem in practice and that `auto` can be beneficial to postpone materialization.
  - Hubert stated that runs into problems with lifetime.
  - Tom asked Mark about applicability to P1072.
  - Mark responded, yes, [P1072](http://wg21.link/p1072) explicitly mentions Abseil's `StrCat` and is instrumental
    to achieving desired performance.
  - Jorg asked for more feedback on whether `wconcat`, `u8concat`, etc... should be provided.
  - Corentin replied, yes please.
  - Tom responded yes, I'd like to hear reasons not to provide them.  As long as the feature remains locale
    independent, there are no encoding related concerns.
  - Peter reiterated, for any locale stuff, let some wrapper type deal with it.
  - Jorg added, or, if you want locale sensitive stuff, use `std::format`.
  - Peter mentioned it would be good to update the paper with benchmarks comparing `concat` and `std::format`.
  - Peter asked if `concat` needs to be concerned with `char_traits` and `allocator`?
  - Zach stated that "allocators are why we can't have nice things"; ignore them.
  - Tom stated that the alternative is to pass an allocator as an argument.
  - Hubert suggested relying on independent type deduction for each argument, no conversions.
  - Zach asked for clarification; calling `concat(1,0)` produces `"10"`?
  - Jorg responded, yes.
  - Peter asekd about allowing the result type to be an explicitly specified template parameter?  This would allow
    supporting multiple string types without requiring deduction of the value type.
  - Jorg expressed a preference for the option of passing an empty string as the first argument.
  - Zach stated that LEWG will ask about constraints on the function.  Where does SFINAE happen?
  - Jorg commented that he came into this meeting only intending to support `std::string` and things easily
    convertible to `std::string_view`.  Support for other types will require constraining the value type across
    all arguments.
  - Tom asked for poll requests.
  - Corentin suggested, do we want to support unadorned numeric conversions as arguments?
  - Poll: Do we want to restrict arguments to strings, characters, and types convertible to strings.

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   3 |   4 |   1 |   0 |   1 |

  - Jorg explained his against vote; It is really convenient to be able to simply convert integers and there is
    lots of usage experience in Google and Abseil.
  - Corentin asked if a better `to_string` would change his vote.
  - Jorg responded, pobably not as it wouldn't be as convenient.
  - Peter stated that wrappers let us add features incrementally.
  - Jorg mentioned that Abseil's `StrCat` provides some converters (e.g., for hex), but they are rarely used.
  - Victor stated he would like to see `concat` merged with an improved `to_string`.
  - JeanHeyd stated that Sol2 provides a number of adorning types but that users don't like them.


# January 23rd, 2019

## Draft agenda:
- Peter Bindels will present his work on a simple 2D graphics library.
- Discuss Steve's latest draft of the SG16 rubric.
- Discuss Tom's latest draft of the char8_t remediation paper.

## Meeting summary:
- Attendees:
  - Bryce Adelstein Lelblach
  - Corentin Jabot
  - JeanHeyd Meneide
  - Michael Spencer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Victor Zverovich
  - Zach Laine
- Peter Bindels presented his work on a simple 2D graphics library.
  - https://github.com/dascandy/pixel
  - Peter summarrized applicability to SG16:
    - In a graphics API, what is the first thing you want to put on the screen?  Text of course!
    - Putting text on the screen requires Unicode support for the library to be generally usable.
    - A text type handling Unicode is therefore needed.
  - Michael stated that how to display text is not part of text processing and therefore not part
    of SG16 scope, but rather fits into SG13 scope.  SG16 won't standardize
    [harfbuzz](https://www.freedesktop.org/wiki/Software/HarfBuzz).  The question for SG16 is,
    how to accept text.  
  - Zach suggested the perspective that, give me a sequence of code points and I will render them.
    The Unicode line break algorithms handle layout pretty well.
  - Peter noted that the bidirectional algorithm is needed as well.
  - Zach added that normalization probably isn't required though.
  - Tom asked if Peter wanted to support italic, bold, or underlined text.  A long and contentious
    debate had been on-going on the Unicode Consortium mailing list regarding whether Unicode should
    enable encoding stress indicators like these.  See the threads at
    https://unicode.org/pipermail/unicode/2019-January/007313.html and
    https://unicode.org/pipermail/unicode/2019-January/007434.html.
  - Peter responded, no, plain text is sufficient.
  - Zach stated that stress indicators are handled by fonts and controlled by markup.
  - Tom elaborated, the question was intended to discover if Peter's needs are limited to plain text
    or whether a higher level of markup is needed.  Would there be a desire to standardize some kind
    of markup?
  - Steve suggested that inline markup could be supported.  Or not supported as desired.
  - Zach added that text display is relatively simple once decoding, line breaks, and bidirectional
    support are enabled; fonts take care of the rest.
  - Steve stated that our immediate goals are low level; provide code point and EGC decoding support.
  - Peter summarized, so we're on the right track working to get new types into the standard.
  - Tom agreed, new types and new algorithms.
  - Steve added, and views.
  - Peter, changing topics, Christopher DiBella has been asking for SG20 how to educate people about
    Unicode.
- [P1253R0: Guidelines for when a WG21 proposal should be reviewed by SG16, the text and Unicode study group](http://wg21.link/p1253r0)
  - Tom asked, do we want to present this to the various WGs or just the WG chairs?
  - Zach stated he was most concerned about EWGI and LEWGI.
  - Corentin suggested presenting to LEWGI, WG chairs, and mentioning it at plenary.
  - Bryce suggested that we could present it at an evening session and could arrange presentation at
    EWGI and LEWGI at Kona.
  - Tom stated he would follow up with WG chairs
    \[Editor's note: Tom did later reach out to the EWG, LEWG, CWG, LWG, EWGI, and LEWGI chairs to ensure that
    1) the chairs agree with the guidelines, and
    2) are willing to direct proposal authors to SG16 when discussion touches on the topics discussed in the paper.

    Several chairs have indicated their agreement so far.  Tom  also requested presenting the paper to EWGI and
    LEWGI in Kona.\]
  - Peter mentioned that there may be examples where a filename may be mutated on open (for normalization purposes)
    such that attempts to reopen the file by the mutated name fail.  We should 1) verify that this can happen and,
    if so, 2) update the paper to mention it.
  - Tom advised that, while we're all participating in WG discussions, that we continue to look for additional
    guidelines to be added.
- Kona pre-planning:
  - Tom mentioned that he was not intending for SG16 to meet in Kona, but it looks like we'll have a quorum after
    all, and papers to discuss, so we will plan to meet.
  - Steve mentioned he has a paper discussing transliteration: [P1439R0: Charset Transcoding, Transformation, and
    Transliteration](http://wg21.link/p1439r0).
  - Tom mentioned that Martinho and Hana have papers for us as well.
    - [P1139R1: Address wording issues related to ISO 10646](http://wg21.link/p1139r1)
    - [P1433R0: Compile Time Regular Expressions](http://wg21.link/p1433r0)
  - Peter asked why Hana's paper is targeting SG16.
  - Zach responded that the Unicode standard specifies algorithms for regular expressions.
  - Bryce suggested SG16 may want to review Hana's paper before LEWGI does.
- `char8_t` remediation:
  - Peter asked if anyone had searched for uses of `u8R`.
  - Tom replied, no, I didn't think to search for `u8` raw literals.
  - Peter mentioned having found one in the wild.
  - Victor searched Facebook and found 3 uses.
  - Tom remembered that Victor previously mentioned approximately 1000 `u8` literals in Facebook code.
  - Victor confirmed, mostly in Facebook code, mostly in tests.
  - Zach commented that he mostly uses `u8` literals in tests as well.
  - Steve stated that Chromium has a number of uses of `u8R` literals.
  - Peter added that a December 2017 snapshot of the code base he works on had 27 uses of `u8` literals, all of
    which were in tests.  Probably has some more now, but not a lot.
  - Tom wondered why hits tend to be in tests.  Perhaps library authors test things that users just don't use?
  - Zach responded that real world uses of libraries tend to use data in strings, not string literals.
  - JeanHeyd echoed Zach, data tends to come from databases or files.
  - Steve stated that most `u8` literals in the code bases he works on are in SQL queries or test code.  They're
    still working to get off of C++03 code and have only mostly been using Unicode internally in the last 10 years.
  - Zach, stated that any changes from the `char8_t` proposal that would result in silent behavioral changes must
    be made ill-formed and that the remediation paper covers that.  We need to be careful when discussing the
    remediation paper that we don't blow up concerns that aren't real.  For example, with concerns that some code
    bases might be badly impacted.
  - Tom asked Zach how he was feeling about code like `std::string(u8"text")` now as he had previously expressed
    concern.
  - Zach responded that he was previously concerned about the amount of breakage, but based on anticipated impact
    and the fact that existing uses will now be ill-formed, no longer concerned.
- Zach asked for clarity regarding guidance SG16 gave proposal authors in San Diego regarding encoding aware
  interfaces.
  - Zach explained, in our [response](http://wiki.edg.com/bin/view/Wg21sandiego2018/D0881R3) to
    [P0881R3: A Proposal to add stacktrace library](http://wg21.link/p0881r3), we said that file names should be
    treated as just a bag of bytes.  But in our [response](http://wiki.edg.com/bin/view/Wg21sandiego2018/P1275R0)
    to [P1275R0: Desert Sessions: Improving hostile environment interactions](http://wg21.link/p1275r0), we
    argued for an interface matching `std::filesystem::path` that exposes content in multiple encodings.
  - Corentin argued that the responses are consistent.  In both cases, the content is stored as a bag of bytes,
    but in the latter case, interfaces are available to offer it in an encoding suitable for display.
  - JeanHeyd expressed a preference for just exposing bytes and leaving display issues up to the consumer.
  - Zach stated that the display forms encourage errors; programmers try to round-trip the names and that doesn't
    necessarily work.
  - Tom stated that it is good for the standard to provide encoding aware interfaces, otherwise programmers will
    re-invent them inconsistently.
  - Corentin observed that implimentations can provide higher quality interfaces because they can rely on platform
    specific behavior.
  - Tom reported having recently searched for uses of `u8string` and `generic_u8string` on Github and found
    only incorrect uses.  For example, cases where the programmer passed the result of `generic_u8string` to `fopen`.
  - Steve said he would be in favor of deprecating the `std::filesystem::path` `u8string` and `generic_u8string`
    member functions for C++23.
  - Tom suggested we could deprecate them in favor of new names that indicate they are for display only.  But asked
    if Zach thought they should exist at all.
  - Zach responded, no, they shouldn't exist as members of `std::filesystem::path`.  Rather, we should have better
    separation of concerns and an independent interface for translating file names to displayable strings.
  - Corentin opined that it is better for programmers to have to think about encoding issues.
- Tom verified that we'll keep with the new meeting time slot for the forseeable future and that the next meeting
  will be February 13th.


# January 9th, 2019

## Draft agenda:
- Preparation for the Kona pre-meeting mailing deadline on 1/21.
  - Review the SG16 rupric assuming a draft is available.
  - Review the char8_t remediation paper assuming a revision is available.
  - Review other papers requiring an update for Kona (P1041, P1097).

## Meeting summary:
- Attendees:
  - Cameron Gunnin
  - JeanHeyd Meneide
  - Mark Zeren
  - Michael Spencer
  - Steve Downey
  - Tom Honermann
  - Victor Zverovich
  - Zach Laine
- Tom stated that he was unable to get a revision of the `char8_t` remediation paper ready for this
  meeting, so no further discussion on it for now.
- We then started reviewing Steve's [draft SG16 rubric](http://www.open-std.org/pipermail/unicode/2019-January/000195.html).
  - Victor asked about locales as he and Howard have been working on chrono updates that add overloads
    based on locale.
  - Tom said, yes, bring to SG16 anything involving locales.
  - Zach expressed a preference for just those locale features that relate to Unicode.
  - Tom stated a preference for having a chance to offer our expertise; to help ensure appropriate use
    of locales.
  - Michael asserted that we don't want new Unicode stuff dependent on `std::locale`.
  - Zach observed that it is very hard to write portable code that uses `std::locale` due to implementation
    defined things.  For example,
    - the set of locales is not specified.
    - even the "C" locale is not portable.
  - Tom suggested that the language regarding "requires review" by SG16 be softened as we don't have standing
    to actually require review.
  - Zach disagreed and offered the perspective that this paper should be adopted by the LEWG and EWG chairs
    with the expectation that the chairs will enforce review requirements.
  - Tom expresseed enthusiasm for that perspective; this paper should be targeted to LEWG and EWG to get their
    buy-in.
  - Tom asked about the SG-7 rubric in the hopes that we could compare/contrast with it.
  - Michael located it and provided a link:
    - http://wiki.edg.com/pub/Wg21sandiego2018/SG7/d1354r0.html?twiki_redirect_cache=c261eaeb64220cb36ab24bdb6fb29d4c
  - Tom suggested we should have a section on text containers and string builders.
  - Zach asked if we care about string builders.  If a string builder is used in such a way that it slices code
    unit sequences, isn't that just an incorrect use of the builder?
  - Tom stated he wants to catch any new operations that are problematic for some encodings.  For example,
    reliance on broken interfaces like `std::ctype::widen`
  - Cameron suggested we're interested in any new overloads involving Unicode types.
  - Zach proposed adding a section detailing encoding assumptions.
  - Tom agreed and suggested that can appear in the text encoding section; we need to make it explicit that char
    based values of unknown origin are assumed to have execution encoding.
  - Zach disagreed with the assumption of execution encoding stating that they should instead have an unknown encoding
    and their contents should only be forwarded and operated on generically (e.g., as a bag of bytes), not examined
    as having data in any particular encoding.
  - Tom challenged this noting that reasonable assumptions can be made.  On Windows, execution encoding matches the
    system code page, on POSIX it corresponds to the `LANG` or `LC_CTYPE` environment variables, and is generally
    ASCII elsewhere (except z/OS).
  - Zach noted that assumption doesn't work for file names.
  - Tom agreed that filenames are special; they don't have a known encoding.  But C++17 at least offers `std::filesystem`
    with means to get a filename in a displayable format via the `*string` and `generic_*string` member functions of
    `std::filesystem::path`.
  - Zach asserted those member functions are a trap; the names retrieved via those member functions don't necessarily
    round trip.
  - Michael observed that programmers need to be able to display file names and, if the standard doesn't provide a way
    to do it, programmers will do it themselves, probably badly.
  - Steve noted that file names may not be presentable at all
  - Michael reiterated that we need interfaces that do the right thing easily; e.g., to create a display name for a file
    in something other than `std::filesystem::path`.
  - JeanHeyd observed that some of these problems would go away with a new I/O layer that uses `std::filesystem::path`
    instead of `const char*` interfaces.
  - Steve noted that we can't replace the OS interfaces though.
  - Tom stated that we need to update the paper to require consultation with SG16 for anything involving file names.
- [P1378R0: std::string_literal](http://wg21.link/p1378r0)
  - JeanHeyd provided a link to an updated draft revision of the paper:
    - https://thephd.github.io/vendor/future_cxx/papers/d1378.html
  - JeanHeyd introduced the motivation; to provide means to guarantee that a string literal is used in invocations
    of `std::embed` in order to enable dependency discovery in build systems.  Additional motivation is to provide
    means to avoid unintended array-to-ponter decay and to handle string literals with embedded null characters without
    having to depend on deduction via array reference in order to obtain the actual array size of the literal.
  - JeanHeyd acknowledged that the proposal changes the type of all string literals in ways that are unlikely to
    be acceptable.
  - Michael observed that the proposed design doesn't actually meet the motivation requirements for `std::embed` since
    the proposed type is copyable and therefore can be produced by many kinds of expressions, not just literals.
  - Steve suggested another motivation: requiring string literals for things like format strings and SQL; requiring
    a literal would avoid the possibility of consuming user provided input that could be used as an attack vector as
    in SQL injection attacks.
  - Zach observed that immediate (`consteval`) functions can help in this regard since they can't consume run-time
    input by design.
  - Tom asked about a different implementation strategy; making all of the class constructors private and befriending
    a UDL.  This would ensure the class could only be constructed by calling a UDL (assuming copy constructors are
    deleted).
  - Michael suggested the constructors could also use compiler magic to require construction via a literal.
  - Steve noted that having the size of a string literal readily available would be useful.
  - Michael noted that this design impacts type deduction for `auto` declared variables and template parameters.
  - Zach suggested that two-step conversion as would be required for backward compatibility would be problematic.
  - JeanHeyd responded that any number of builtin implicit conversions are already permitted.
  - Tom wondered if the number of conversions might impact overload resolution.
  - JeanHeyd suggested the design might be useful to limit when error handling and encoding validation would be
    necessary for `std::text`.
  - Zach countered that string literals can form ill-formed code unit sequences.
  - Zach acknowledged that the ability to avoid `strlen` could be a big deal.
  - Michael asserted that the motivational use cases can largely be met with immediate (`consteval`) functions.
  - JeanHeyd provided an additional motivation; comparison between string literals.  Today, whether `"foo" == "foo"`
    is unspecified.  The proposed `std::string_literal` could make such comparisons work as expected.
  - Mark asserted that an implementation is needed to evaluate backward compatibility impact.
  - Mark noted having previously had a desire to determine if a pointer pointed to a string literal; to avoid
    storing the string contents.
  - Zach and Tom both expressed having used or encountered string pool classes that exist to collapse matching strings
    to a single copy.
- WG21 Direction group [response to P1238R0: SG16: Unicode Direction](http://www.open-std.org/pipermail/unicode/2019-January/000196.html)
  - Steve summarized the response.
  - Tom noted that the DG did not comment on the constraints listed in the paper.
  - Mark noted the DG request to clarify scope.
  - Zach stated that we need an elevator pitch and suggested: We want all Unicode algorithms available via standard
    interfaces for C++23.
- Tom announced that the next meeting will start an hour later than usual.


# December 19th, 2018

## Draft agenda:
- Continue discussion of char8_t remediation for backward compatibility impact.
  - Discuss pros/cons of keeping u8 literals char based and introducing new char8_t based U8 literals.
- Review P1072 following San Diego LEWGI feedback.

## Meeting summary:
- Attendees:
  - Bryce Adelstein Lelblach
  - JeanHeyd Meneide
  - Mark Zeren
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
- Continued discussion of char8_t remediation for backward compatibility impact.
  - Tom introduced the discussion topic.  One approach to minimizing backward compatibility impact
    would be to restore `u8` literals being `char`-based and to introduce a new `U8` literal prefix
    for `char8_t` based UTF-8 literals.
  - Mark suggested following up with Google folks to determine if this would address their concerns.
  - Tom stated he talked to Chandler following the San Diego vote.  Concerns expressed were that the
    potential backward compatibility impact exceeded the benefits.
  - Tom asked for pros and cons for a new `U8` literal prefix.
  - JeanHeyd was first to note the obvious primary benefit, avoids backward compatibilty issues.
  - Tom agreed, but added that P0482 does have other minor breakage; the changes to the return types
    of the `u8string` member functions of `std::filesystem::path`.
  - JeanHeyd pointed out that the visual difference between `u8` (lowercase) and `U8` (uppercase) is
    subtle and bad for readability.
  - Bryce agreed and pointed out that MISRA forbids identifiers that look similar.
  - Bryce further stated that use of `u` and `U` for `char16_t` and `char32_t` literals was a mistake
    for the same reason.
  - Mark mentioned a pro, this approach preserves investment in any increased use of `u8` literals
    in code over the next few years before migration to C++20.
  - Bryce suggested that compiler warnings could be added to help educate programmers about the change
    when compiling in pre-C++20 language modes.  This still depends on compiler upgrades of course.
  - Tom agreed and noted that Clang trunk already issues such a warning when invoked with
    `-Wc++2a-compat`.
  - Mark asked if a cast or similar approach for converting `u8` literals to `char`-based types doesn't
    suffice.
  - Tom responded that Zach expressed a desire for existing code to continue working at our last
    meeting.
  - Tom asked what adoping an additional literal prefix would mean for messaging.  What would we be
    telling programmers going forward?  We could deprecate `u8` literals and promote `U8` going
    forward.
  - JeanHeyd responded that deprecation doesn't really help to move programmers towards use of `char8_t`.
    He'd prefer to break things, get over the migration hump, and keep a cleaner design.
  - Mark asked why the `as_char` approach suggested in the draft paper doesn't suffice.
  - JeanHeyd responded that it requires markup, so existing code requires changes.
  - Mark pondered, a new prefix does kind of fix everything.  It doesn't have to be `U8`, we could use
    `utf8` or similar.
  - JeanHeyd suggested we could introduce new prefixes for all of UTF-8, UTF-16, and UTF-32 in order to
    maintain symmetry and to address the subtle `u` vs `U` concerns.
  - Tom suggested another pro; a new prefix avoids potentially forking the language by unintentionally
    encouraging use of a `-fno-char8_t` option as has happened with `-fno-rtti` and `-fno-exceptions`.
  - Mark asked where we're at with proposing `char8_t` to WG14.
  - Tom responded that he would like to get a proposal in front of WG14 at their October 2019 meeting
    in Ithaca.  In addition, he'd like to have proposals ready for our other proposals targeting core
    language features:
    - [P1097](http://wg21.link/p1097) - "Named character escapes"
    - [P1041](http://wg21.link/p1041) - "Make char16_t/char32_t string literals be UTF-16/32"
    - Source file encoding tags (no proposal yet).
  - Tom added another pro, or con, depending on perspective; a new prefix maintains the ability to
    continue writing UTF-8 based applications with `char`-based types.
  - Mark opined that moving away from `char` aliasing issues is compelling.
  - Steve noted that UTF-8 in `char`-based types often seems to work, but works for the wrong
    reasons.  For example, UTF-8 encoded source files compiled as "8-bit ASCII" such that the UTF-8
    code units just get copied from the source file.
  - Tom asked about messaging again, what message are we sending to library authors?  Do they write
    their UTF-8 based interfaces against `char` or `char8_t`?  How do they choose?
  - Mark observed that this isn't a new problem.  Library authors code against `std::string` today
    and it isn't a universal string type or a great type for Unicode.  We'll have similar concerns
    with the introduction of `std::text` vs `std::string`.
  - Tom concluded, sounds like templates will be the way to go.
  - JeanHeyd commented that views help.  For example, `text_view` can effectively type erase the code
    unit type.  But what does one assume for encoding for `char`?
  - Tom responded that the execution encoding must be assumed per existing precedent in the standard.
  - Mark concluded that he doesn't see a way out of the `char` vs `char8_t` problem.  But, with `char8_t`
    being available, we'll get experience using it that will inform future library efforts.  In the short
    term, being able to use either `char` or `char8_t` is advantageous.
  - Peter chimed in from chat (due to a non-functioning microphone):
    - "looks like my mic is completely broken. From what I can tell this is like the uptake of uint8_t,
      it takes some time but over time everybody learns that these types have a given fixed meaning and
      others are a :shrug: type"
  - Tom presented a few polls.
    - Poll 1: Add defined-as-deleted overloads for `operator<<` for `basic_ostream<char, ...>` specializations.

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   3 |   3 |   0 |   0 |   0 |

    - Poll 2: Allow deprecated `std::filesystem::u8path` to be called with sources with `char8_t` value type.

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   2 |   3 |   0 |   1 |   0 |
    - - Peter explained his against vote; this maintains working around something that we don't really
          want to work in the first place.

    - Poll 3: Restore `char`-based `u8` literals and introduce new `char8_t` based literals with a new prefix.

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   1 |   3 |   1 |   1 |   0 |
    - - Bryce explained his against vote; we'll need to converge on a very short prefix, 2 characters at
        most.  That seems unlikey.
      - JeanHeyd commented that he still prefers to go with a solution that pushes the community in a
        new and consistent direction.  `u8` literals aren't widely used, so we still have time to course
        correct.
      - Mark asked if tooling could be used to fix existing code by converting `u8` literals to ordinary
        literals encoded with escapes.
      - Tom responded that we discussed tooling possibilities at the last meeting.  Specifically Zach's
        suggestion that this could be a good test for Titus' goals for tooling.

    - Poll 4: Assuming `u8` literals remain `char8_t` based, allow `char` arrays to be initialized with `u8` string literals.
      - Tom stated that the reason to consider this is that the `as_char` approach doesn't work for
        array initialization.
      - Bryce stated he wanted more time to think about this.
      - Mark agreed with wanting more time.
      - Poll not taken.
- Review P1072 following San Diego LEWGI feedback.
  - Mark provided a summary of changes:
    - No buffer moving features; feedback from San Diego was negative regarding that due to exposure of
      implementation details.
    - `resize_default_init()` resizes the string such that the added content is default initialized.
      Failure to write to the added elements results in undefined behavior.
    - This approach matches Google's existing implementation.
    - This approach is compatible with existing allocators.
    - libc++ is already using this approach as part of its `std::filesystem` implementation to remove
      an allocation.
    - This doesn't preclude a buffer migration feature in the future.
    - The paper establishes that `basic_string` is allocator aware.


# December 5th, 2018

## Draft agenda:
- Draft guidelines for other WGs and SGs to request SG16 review.
- char8_t remediation for backward compatibility impact.
- Review P1072 following San Diego LEWGI feedback.

## Meeting summary:
- Attendees:
  - Bryce Adelstein Lelblach
  - Cameron Gunnin
  - Corentin Jabot
  - Florin Trofin
  - JeanHeyd Meneide
  - Mark Zeren
  - Markus Sherer
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Draft guidelines for other WGs and SGs to request SG16 review.
  - Tom introduced the topic.  Bryce had suggested that SG16 produce a rubric detailing guidance
    for when other WGs and SGs should consult SG16.  SG7 recently produced such a document.  Tom
    felt this was an excellent idea and is now bringing it before SG16 for discussion.
  - Tom first asked Bryce where SG7's rupric can be found.
  - Bryce replied that it will be in the San Diego post-meeting mailing.
  - Tom then asked for suggested guidance.
  - Steve suggested a simple litmus test; "if it smells like Unicode..."
  - Corentin mentioned having discussed this with Titus in San Diego and suggested that anything
    having to do with text processing should be sent our way.
  - Bryce asked about locales and it was agreed that Unicode has locale dependencies.
  - Peter mentioned the {fmt} library; code units vs code points?
  - Tom replied that we discussed {fmt} with Victor in SG16 on several occassions.
  - Bryce asked if {fmt} is in C++20 and whether SG16 has any concerns about it.
    \[Editor's note: not yet, but it passed LEWG review in San Diego\].
  - Zach replied that it is certainly no worse than what we have now.
  - Mark commented, bird in hand... even if we had issues with the {fmt} library, there is no
    competing proposal.
  - Corentin mentioned that {fmt} does not yet handle `char16_t` and `char32_t`, but can be extended
    later.
  - JeanHeyd elaborated, template overloads are present, but formatting strings must be `char`
    or `wchar_t` at the moment.
  - Zach suggested a requirement; that we need to reserve the right to explicitly specialize standard
    library templates that might be instantiated by users with `char8_t`.
  - Tom asked for a volunteer to identify such templates.
  - Zach volunteered.  Hooray for Zach!
  - Steve suggested that anything involving command lines, file names, and environment variables should
    be sent our way.
  - Mark added, any kind of encoding.  Including source encoding.
  - Tom asked, do we want SG13 (HMI) members consulting us for text input and presentation issues?
  - Steve replied, when they get to that point, yes.
  - Tom asked for a volunteer to draft the rubric paper.
  - Steve volunteered.  Hooray for Steve!
- char8_t remediation for backward compatibility impact.
  - Tom gave a brief introduction and pointed the group at a rough draft paper posted to the mailing list
    (http://www.open-std.org/pipermail/unicode/2018-December/000180.html).
  - Time was given for those who had not yet seen it to quickly scan it.
  - Steve commented on the proposed change to make ostream inserters for `char16_t` and `char32_t`
    ill-formed; for anyone actually relying on printing pointer values, a fix should be easy, add a
    cast to `void*`.
  - Corentin wondered if anyone actually does `std::cout << u8"text"`.
  - Zach observed that someone could conceivably want to use the ostream inserters to print `char16_t`
    values formatted as hex integers, say when dumping UTF-16 code units for diagnostic purposes.
  - Steve asked if it would be problematic to allow `std::string` to be constructed with `char8_t` based
    data.
  - Zach responded that he didn't see any harm.
  - Peter chimed in that `std::string` always holds UTF-8 in the code base he works on.
  - Tom stated that supporting `std::string` interoperability with `u8` literals would require a lot of
    overloads for the `char` based specialization of `std::basic_string`.  Implementors would not like
    that.
  - Zach asserted that he wants, somehow, to be able to construct `std::string` objects initialized with
    `u8` literals.
  - Tom asked if using a factory function would suffice.
  - Zach responded that would require updates and therefore doesn't address existing code.
  - Markus advised thinking of `std::string_view` in addition to `std::string`.
  - JeanHeyd asked about allowing `std::u8string` to be convertible to `std::string`.
  - Tom stated he thought that might allow most existing code to just work.  But, would we really want
    that?  Implicit conversions are often undesirable.
  - Peter responded that he thought so, yes.  Existing code mixes UTF-8 with `char`.
  - Corentin observed that implicit conversion from `std::u8string` could lead to mojibake.
  - Zach acknowledged that `std::string` doesn't guarantee any encoding.
  - Peter asked about the possibility of making it UB for `std::u8string` to contain non-UTF-8 data.
  - Zach requested not adding encoding guarantees for strings.
  - Peter responded, it doesn't actually work anyway since you couldn't update a string without
    introducing UB.
  - Tom asked if the UDL approach to providing UTF-8 data in `char` via `u8` literals was realistic.
  - Zach stated we shouldn't be suggesting macros as solutions.  \[Editor's note, macros are not required
    to create a solution that works for C++17 and C++20, but source code changes are required\].
  - Tom asked if use of `-fno-char8_t` is a valid option noting that it forks the language.
  - Zach suggested, perhaps this is our first good opportunity to put tooling to use as part of a C++20
    migration story.
  - Corentin observed that it should be easy to use `clang-tidy` to update code.
  - JeanHeyd asked if `char8_t` could implicitly convert to `char`.
  - Corentin stated that he wants conversions to be explicit.
  - Tom mentioned that the draft paper is intended to tell a migration story.
  - Markus explained that he felt the economics are not right.  The current situation puts the burden of
    addressing breakage on many programmers.
  - Zach suggested adding tooling automation to the paper.
  - Tom said he could add `clang-tidy`, what else should be mentioned?
  - Zach stated he'd like to see compilers do fix-ups themselves.
  - Corentin observed that implementors are unlikely to have something in place in the necessary time frame.
  - Tom asked about experimentation.
  - Peter stated his code base isn't using `u8` literals today and won't be able to.
  - Markus observed that not all code is equally modifiable.  For example, Google's code base has a lot of
    Google specific code, but also uses a lot of third party code.  Updating the third party code and
    potentially maintaining differences from upstream, is more difficult than updating Google's own code.
  - Tom suggested a C++17 compatibility library could be made available that implements some of the
    remediation approaches noted in the draft paper.
  - Bryce asked about the possibility that the `char8_t` proposal might be re-litigated due to backward
    compatibility concerns.
  - Tom replied, sure, anything is possible.
  - Bryce suggested adding data about expected brekage to the remediation paper to avoid scaring people.
- Peter requested time in SG16 for presenting and collecting feedback on a simple 2D graphics library
  he has been working on.


# October 17th, 2018

## Draft agenda:
- char8_t: Markus' concerns, motivation, type safety, Unicode sandwich, most C++ code is yet to be written, transition story.
- Code points, EGCs, or explicit ranges for text views/containers?
  - How to decide? Pick a direction now? Write a pros/cons paper for the committee?

## Meeting summary:
- Attendees:
  - Artem Tokmakov
  - Cameron Gunnin
  - JeanHeyd Meneide
  - Mark Zeren
  - Markus Scherer
  - Martinho Fernandes
  - Sergey Zubkov
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- [Issue #30: Unclear behavior for octal and hex escape sequences in Unicode character and string literals](https://github.com/sg16-unicode/sg16/issues/30)
  - Tom explained the current situation; [CWG#2333](http://wg21.link/cwg2333) tracks this
    issue.  CWG discussed at their August 2017 teleconference and decided that numeric escape
    sequences should be ill-formed in UTF-8 character literals.  Mike Miller offered to reconsider
    the issue if requested by SG16.
  - Markus mentioned the utility in using numeric escapes to create ill-formed strings for testing
    purposes.
  - Markus also presented an alternative possibility, that numeric escapes only be ill-formed if
    used to encode a code unit value that is never valid in a UTF string, e.g., `0xff`.
  - Markus additionally noted that there is a distinction between Unicode strings (may contain
    ill-formed contents) and UTF strings (must be well-formed).
  - Zach asserted that the ability to use numeric escapes is more important than preventing
    encoding of ill-formed UTF sequences.
  - Tom noted that the current CWG resolution seems evolutionary given that it contradicts existing
    practice.
  - Markus noted a further benefit, maintaining consistency with languages like Java.  Additionally,
    he explained that some logging libraries write strings with non-printable characters replaced
    with escape sequences and that the ability to copy and paste those strings verbatim into code
    is useful.
  - Tom noted an additional use case; strings encoded as Modified UTF-8.  Modified UTF-8 requires
    use of escapes to encode U+0000 as an overlong two-byte sequence.
  - Markus added that the same use case applies to creation of CESU-8 strings; escape sequences are
    needed for the individual encoding of UTF-16 surrogate pairs.
  - Tom stated that it is useful to embed a null terminator with `\0`, though it would still be
    possible to do so using `\u0000`.
  - Mark observed that implementations can warn if a literal that contains numeric escape sequences
    produces an ill-formed UTF string.
  - Poll: Continue to allow hex and octal escapes that indicate code unit values, requiring only
    that they fit into the range of the code unit type.

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   8 |   1 |   0 |   0 |   0 |
    
- char8_t:
  - Zach started the discussion by noting that use of `char8_t` does not help to enfore
    preconditions; ill-formed UTF-8 can appear in sequences of `char8_t` just as it can in sequences
    of `char`.  How does `char8_t` help?
  - Mark acknowledged that preconditions can always be violated.
  - Tom offered `make_text_view` and UDLs as examples.  `char8_t` enables writing generic functions
    that work with ordinary and UTF-8 string literals.
  - Zach summarized, I see, it allows authors of overload sets to differentiate behavior.
  - Markus chimed in, starting to see the motivation for `char8_t`; generic code can't distinguish
    encodings unless it is represented in the type system.
  - Markus further noted that the standard library has a high percentage of generic code relative to
    code outside the standard.
  - Tom agreed, but noted there is more focus on generic libraries now than in the past and that the
    committee is working hard to improve support for generic programming as exemplified by Concepts.
  - Tom mentioned that we have multiple encodings we have to support.
  - Markus acknowledged the dilemma; many other languages have settled on a single internal encoding,
    but C++ supports multiple encodings and there is no clear dominant one across the industry.
  - Mark added that there is considerable baggage with `char` and the implementation definedness of
    the execution encoding.
  - Markus acknowledged the existence of many incompatible string types in C++ that are all similar
    in intent.
  - Tom stated that Concepts helps to bring these different string types together such that they
    can be supported by generic code.
  - Markus observed that the `char8_t` proposal changes existing behavior.
  - Mark noted that `u8` literals aren't used much in C++.
  - Markus mentioned that Google uses `unsigned char` and ensures use of UTF-8 internally.
  - Tom responded that there is a backward compatibility story that is aided by C++20 support for
    class types as non-type template parameters as proposed in [P0732](http://wg21.link/p0732).
- Code points vs grapheme clusters:
  - Martinho lead the discussion by expressing concern that grapheme cluster boundaries are not
    stable.  The situation with Swift today is that behavior depends on the version of ICU installed
    on the system.  Behavior is therefore non-portable.
  - Mark mentioned that we have a similar issue with the timezone database and `<chrono>`.  Behavior
    depends on which version of the database is installed.
  - Tom acknowledged the concern; we won't have portable grapheme breaking in C++ either.
  - Markus provided a link to a recent document authored by Mark Davis and noted a limitation imposed
    by the instability of grapheme cluster boundaries; stored EGC indexes are invalidated when
    changing Unicode versions.
    - https://docs.google.com/document/d/1wuzzMOvKOJw93SWZAqoim1VUl9mloUxE0W6Ki_G23tw/edit
  - Zach asked, as someone without a lot of end user experience, how often do programmers make poor
    choices regarding handling of Unicode text?
  - Steve responded that he sees bug reports frequently where programmers inadvertently sliced
    grapheme clusters.
  - Martinho provided links to a couple of example defects:
    - https://bugs.swift.org/browse/SR-3582
    - https://stackoverflow.com/questions/26862282/swift-countelements-return-incorrect-value-when-count-flag-emoji
  - Tom asked, so how do we make a decision about how to proceed.
  - Martinho countered that we don't need to yet.
  - Steve chimed in with, how do we make them less scary?
  - Mark responded with a question, how are things going to look?  new types on top of `std::string_view`
    and `std::string`?
  - Zach provided a brief overview of how Boost.Text handles grapheme clusters.
  - Markus asked, does Boost.Text enforce well-formed UTF-8?
  - Zach responded that it encourages, but does not require well-formed UTF-8.
  - Markus mentioned that validation can be expensive.  If you know your input is well-formed, then
    lookups can be optimized without having to decode.
  - Tom described this as a design trade off; validate up front and reap performance benefits later,
    or skip validation and lazily validate later.
  - Markus noted that it is common for programmers to slam content into strings and then validate
    them later.
  - Mark mentioned that [P1072](http://wg21.link/p1072) helps to support that use case.
  - Tom asked, assuming that we standardize a type that enforces well-formedness, is there room for
    standardizing a non-validating type as well?  Or does that become an expert level do-it-yourself
    feature?
  - JeanHeyd advocated an adapter-over-range approach for `std::text`; tags can suppress validation
    when it isn't necessary.
  - Tom observed that it isn't possible to enforce well-formedness on views without introducing
    validation costs.
  - Steve mentioned that adapters over containers make memory allocation someone else's problem, for
    better or worse.
  - Martinho advocated that, if performing validation on container construction, would prefer
    replacement character substitution since throwing gives you nothing.  Invalid input can be used
    as an attack vector; if UTF-8 input is all `0x80`, replacement will triple the buffer size.
  - Zach expressed openness to an adapter approach for Boost.Text.
  - Mark expressed a preference for the adapter approach as it supports underlying containers with
    reference counts or small buffer optimizations.
  - Mark also mentioned that wrapping `std::string` provides a nice transition story.
- Tom then summarized the plan for the San Diego meeting: discussion of the
  [Unicode Direction paper](http://wg21.link/p1238), [P1072](http://wg21.link/p1072), Isabella
  Muerte's [P1275](http://wg21.link/p1275), and then small groups to focus on further proposal
  incubation.


# October 3rd, 2018

## Draft agenda:
- Last meeting before the San Diego pre-meeting mailing deadline on October 8th.
- Review the draft SG16 direction paper that Tom plans to have ready for this meeting and
  the pre-meeting mailing.
- Code points, EGCs, or explicit ranges for text views/containers?
  - How to decide? Pick a direction now? Write a pros/cons paper for the committee?

## Meeting summary:
- Attendees:
  - Artem Tokmakov
  - Corentin Jabot
  - JeanHeyd Meneide
  - Mark Zeren
  - Markus Scherer
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- We started off with a round of introductions in honor of a new first time attendee,
  Markus Scherer, chair of the ICU Technical Committee.
- Tom provided a brief overview of the agenda; to review draft papers discussing
  SG16 direction, to collect feedback, and submit a paper for the San Diego pre-meeting
  mailing that represents the group's consensus on our general direction.
  \[Editor's note: these drafts later became [P1238R0](http://wg21.link/p1238).\]
- Zach raised a concern regarding support for generic interfaces.  The draft paper asked
  whether generic interfaces for Unicode algorithms could reasonably support segmented
  data structures like ropes.  Zack felt segmented data structures are supported naturally
  as long as they provide standard iterators.
- Tom explained that the question was meant more to ask if generic interfaces could provide
  performance that users would expect.  Or whether interfaces specialized for contiguous
  memory would be necessary and, if so, whether they could be used to service ropes.  Perhaps
  it would make sense to have a low level C API wrapped in a generic interface.  This would
  require the low level API to support tracking state (e.g., code unit sequences split across
  segment boundaries).
- Zach expressed concern about giving the impression that we want to provide equivalent
  functionality in C and C++.
- Corentin chimed in that contributing to C isn't something we've talked much about.
- Tom clarified, only when it makes sense.
- Markus noted some experience; prior attempts to provide generic interfaces in ICU
  resulted in performance complaints.  ICU could do more of this, but users are able to do
  it themselves.
- Zach responded that his own performance tests involving arrays of code points vs code point
  iterators on top of code units indicated negligible performance differences.  Table lookups
  dominated.
- Markus commented that performance improvements come about largely due to support for fast
  paths.
- Mark observed that we heard similarly from Swift developers regarding the need to support
  fast paths.
- Markus then asked a fundamental question: why bother standardizing Unicode support?  Why
  not just use ICU?
- Mark responded that programmers continue to struggle with classes of bugs that we could
  potentially minimize, handling of grapheme clusters for example.
- Steve also noted continued mishandling of strings in general.
- Tom mentioned distribution and packaging issues.  Having something provided with the standard
  library helps to sidestep legal obstacles and package versioning problems.
- Corentin commented that programmers need more easy to use functionality, libraries that
  encourage correct use.
- Tom agreed, noting that we want to bring down the learning curve for working with Unicode.
- JeanHeyd added that not all programmers need all of Unicode, some would benefit just by having
  support for encodings built in.
- Changing topics, Mark asked to add a reference to P1072 in the paper, noting its relevance to
  text/string buffer transference.
- Steve asked about some of the terminology in the paper.  Why the inconsistent mention of UTF-8
  vs `char16_t` and `char32_t`?
- Tom explained that this is consistent with the standard where `u8` literals are explicitly
  UTF-8, but `u`, `U`, and other uses of `char16_t` and `char32_t` currently have implementation
  defined encodings.
- Corentin observed that `char16_t` and `char32_t` are explicitly used for UTF-16 and UTF-32
  respectively in the filesystem library.
- Changing subjects again, Tom asked for thoughts regarding the first constraint in the paper,
  that the ordinary and wide execution encodings are implementation defined.  Can we lift that
  constraint?
- Tom went on, Microsoft is working on adding better UTF-8 support to Windows and their
  compiler.  IBM does not provide a publicly available C++11 compliant compiler for z/OS, though
  they do provide Swift on z/OS and that depends on Clang.  IBM doesn't publicly provide Clang
  on z/OS, but it seems they have an internal port of it.
- Markus noted that ICU dropped support for IBM's z/OS, i, and AIX operating systems when
  upgrading to C++11 due to lack of C++11 support in IBM's xlC compiler.
- Corentin mentioned that we're targeting C++23 or C++26 for our work.  What will things look
  like then?
- Changing topics again, Markus commented on ICU's switch to using `char16_t` as the code unit
  type for its internal encoding.  This was challenging due to interoperability issues with
  code that used, and continues to use, `wchar_t` or `uint16_t` for UTF-16 data.  Overloads were
  added to make it eaiser to integrate with code using these types.
- Tom asked to confirm his historical understanding, that ICU used to use a typedef for the code
  unit type that consumers could set to `wchar_t` or `uint16_t` as required for their
  application.
- Markus confirmed that users can still do so, but that the default is now `char16_t` when
  compiling as C++11.
- Zach asked to talk about UTF-8 and type safety.  He was recently surprised when, due to a
  mismatch between the encoding used for a source file (UTF-8) and the encoding the compiler
  used to read that source file (Windows 1252), `u8` string literals didn't have the expected
  contents at run-time.  He concluded (accurately) that he can't depend on `u8` string literals
  containing well-formed UTF-8 text.  This caused him to question his perception of the type
  safety that `char8_t` provides.
- Markus expressed further concerns about `char8_t` leading to the same type interoperability
  issues that were encountered with `char16_t` in ICU.
- Mark noted that we are still lacking deployment results with `char8_t`.
- JeanHeyd described prior experience using a `char8_t` like type to help avoid encoding confusion
  and that it was useful.
- Tom stated that he will add discussion of `char8_t` to the agenda for the next meeting and
  update discussion in the direction paper.
- Changing topics, Markus mentioned a wish list item, that `char` be made unsigned everywhere.
- Mark thought floating the idea would be worthwhile.
- Tom asked Steve about merging the two draft papers.  Steve was favorable to the idea.
- Steve also mentioned that the paper needs to discuss concerns with allocators.  Tom agreed.
- Mark expressed a desire to discuss allocators in San Diego.
- Steve also suggested that the paper address the expected delivery time for features we're
  discussing.  In particular, to make it clear that `std::text` is not targetting C++20.
- Tom agreed.  Mark stated the paper should also address the intended target for existing papers
  in flight.
  

# August 29th, 2018

## Draft agenda:
- SG16 direction.  Where are we heading?  Big picture.
- Code points, EGCs, or explicit ranges for text views/containers?
  - How to decide?  Pick a direction now?  Write a pros/cons paper for the committee?

## Meeting summary:
- Attendees:
  - Artem Tokmakov
  - JF Bastien
  - Mark Zeren
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- With apologies from the editor, this summary writeup was very much delayed.
- Zach started off with an update on Boost.Text.  He noted that implementing the
  Uncode bidirectional algorithm was challenging.  Noone was surprised.
- Tom provided a brief summary for the agenda.  Basically to review our direction and
  confirm common goals and scope.
- JF asked what we have planned for C++20 to which Tom replied that we have a few small
  features in the queue and might otherwise take on some wording cleanup.
- Steve asked about timing for a potential TS and discussion ensued regarding how to
  get usage experience vs the benefits of going straight into the standard.
- Tom proposed a few statements to be considered as axioms, guidelines, questions, or
  possible directives for our work.
- (Axiom) 1: C++ has a long history of supporting non-Unicode encodings; we can't abandon legacy encodings.
  - JF brought up the concept of bridging with a comparison to `std::thread` and `native_handle`.
    E.g., an interface could provide a Unicode centric interface that abstracts support
    for legacy encodings.
- (Axiom) 2: execution and wide execution character encoding will remain run-time
   properties, `char8_t`, `char16_t`, and `char32_t` encodings will remain compile-time
   properties.
  - Tom asserted that legacy compatibility prevents mandating that the execution and wide
    execution encodings be fully known at compile time and noted that they can be changed
    dynamically by calling `setlocale`.
  - Tom also noted that WG14 is considering allowing a program's locale to be dynamically
    changed on a per-thread basis.  See [WG14 N2226](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n2226.htm).
  - Artem asked how much we've been looking at existing locale support.
  - Zach responded that the existing locale support is insufficient to implement some parts
    of Unicode, in particular, support for tailoring.
  - JF mentioned that Javascript internationalization may be a good resource with regard to
    how to map locale information to Unicode.
- (Guideline) 3: Encourage the internal vs external encoding model with UTF-8 as the
  preferred internal encoding.
  - Tom asked if it is reasonable to encourage use of a particular encoding as the internal
    encoding.
  - Zach replied that he feels we must in order to avoid having to perform internal conversion
    rather than (only) conversions at component boundaries.
  - Mark suggested that extensions could enable support for other encodings.
  - Peter emphasized existing advocacy and trends with regard to UTF-8:
    - https://utf8everywhere.org
    - https://w3techs.com/technologies/overview/character_encoding/all
  - Tom asked JF if he could comment regarding how UTF-8 fits into the Apple ecosystem.
  - JF responded that, as long as convenient transcoding interfaces are available, that it
    wouldn't be an issue.
  - Tom asked if restricting access to code units in `std::text` (in order to allow the
    internal encoding to be implementation detail) would break use cases.
  - Zach responded yes, that prevents passing the underlying code unit sequence to C APIs.
    \[Editor's note: this response presumes that the underlying code unit sequence contains
    a nul terminator\]
- (Directive) 4: Improve support for transcoding at program borders (command line, env vars,
  stdin, stdout, text files, network).
  - Zach suggested not focusing on improving this now; let `fmt` deal with I/O; don't
    enhance iostreams.
  - Mark stated that we don't have to fix all of the problems with the standard library.
- (Question) 5: Do `std::text` and `std::text_view` replace `std::string` in new programs?
  - Mark stated no, not as a drop in replacement.
  - Zach noted that we want to continue using `std::string` for simple cases.
  - Tom asked, for new code, do we advocate a preference for `std::text` and `std::string`
    only when needed?
  - Zach stated no, for performance reasons.
  - Tom clarified: that indicates a specific reason to prefer `std::string` in some context,
    but in general, can we advocate use `std::text` unless there is a reason not to?
  - Zach responded that an AAT (Almost Always Text) rule would make sense.
  - Peter asked if it would ever be wrong to use `std::text` instead of `std::string`.
  - Zach replied, no.
  - Peter provided an example by way of `set<text>`.  If `std::text` comparisons are
    expensive (e.g., canonical equivalence vs lexicographical), use as a container element
    may not be desirable.
  - Zach noted that might be a reason to specialize `std::less`.
  - Zach observed that comparison cost is only an issue for relational comparison, equivalence
    is inexpensive if the text is already normalized.
  - Mark summarized, `std::text` provides storage, comparisons need specialized support.
- (Question) 6: How do we manage `std::text` and `std::string` conversions?
  - Tom asked if we need the ability to transfer buffer ownership between `std::string` and
    `std::text`.
  - Mark replied, yes, and that it needs to handle short buffer optimizations, but that this
    is lower priority than making the Unicode algorithms available.
  - Artem observed that `std::string_view` helps here.
- (Question) 7: Where do null terminated strings fit in?
  - Tom asked, can we try to reduce demand for them?  Perhaps propose a string/text type to WG14?
  - Everyone replied, not quickly :)
  - Mark asked if `std::text` needs null termination.
  - Zach replied that it can be provided at the code unit level for C compatibility, but doesn't
    make sense to provide null termination for code point or grapheme cluster sequences.
- (Question) 8: Where do Unicode algorithms fit into the library and are they independent of
  `std::text`?
  - Tom stated a preference that Unicode algorithms are usable with arbitrary string types.
  - Zach agreed stating that we should have code point range/iterator based interfaces as well as
    grapheme cluster range based interfaces.
- (Directive) 9: Adopt useful features from other languages.
  - Tom clarified, for example, named escapes as proposed in [P1097](http://wg21.link/p1097).
  - No disagreement.
- (Directive) 10: Fix existing issues as needed.
  - No disagreement.
- (Question) 11: What role do we take with WG14?
  - Tom asked, the question is really how much time to spend here.
  - Zach stated that engaging with WG14 over `char8_t` and terminology updates makes sense.
  - Mark observed that making Unicode data available via a C API could be useful.
- (Question) 12: What is our target schedule?
  - Steve suggested mostly targeting C++23, not a TS.
  - Zach noted that we need to ensure usage experience and that we have bandwidth limitations.


# July 25th, 2018

## Draft agenda:
- Discuss the Unicode support experience with Swift and WebKit representatives
  (tentative pending their availability).
- Review our issues list and start identifying goals for San Diego.

## Meeting summary:
- Attendees:
  - Artem Tokmakov
  - JeanHeyd Meneide
  - Mark Zeren
  - Tom Honermann
  - Zach Laine
- Tom announced that meeting with Swift developers was postponed due to scheduling conflicts and
  that, in the meantime, we'll focus on interaction with them over email.  \[Editor's note:
  Michael Ilseman and Dave Abrahams responded to the initial set of questions.  Their responses
  are available in the SG16 mailing list archive at
  http://www.open-std.org/pipermail/unicode/2018-August/000113.html \]
- Discussion then proceeded with review of the
  [SG16 issues list](https://github.com/sg16-unicode/sg16/issues).
- [Issue #2: Deprecate `std::ctype`, `std::ctype_byname`, `std::isupper()`, and `std::toupper()`](https://github.com/sg16-unicode/sg16/issues/2)
  - Zach suggested writing a direction paper regarding deprecation policies.
  - Artem, observing that the indicated functions are used by iostreams (e.g., by `std::uppercase`),
    suggested we just go the extra mile and deprecate iostreams to a mixture of approval and
    laughter.
  - Mark suggested that the issue scope be limited to previously identified functions.
  - Tom agreed and renamed the issue (previously "Deprecate text/string/character interfaces that
    are too broken to fix").
  - Zach mentioned that `isupper`, `isnum`, and `isalpha` are definitely broken for Unicode and
    expressed a preference that, if we're going to deprecate them, we should do so early in order
    to encourage replacement.
  - Zach went on to explain that replacements that properly handle Unicode must take locale into
    account in order to do title casing and case mapping correctly.
  - Tom asked for clarification - a code point based `toupper()` doesn't make sense?
  - Zach responded, no; more information is needed.
  - Tom asked, what about `isupper()`?
  - Zach answered, Unicode properties can answer that question, but are insufficient for doing
    case conversions.
  - Tom summarized, the take away is that interfaces in `<ctype>` and `<locale>` are definitely
    broken.
  - Mark added, yup, especially considering that `int` is signed.
  - Artem asked about support for UTF-8, UTF-16, and UTF-32.
  - Mark replied, yup, those are problematic.  Even for `char32_t` due to combining code points.
  - Tom stated this is not a high priority for C++20; no objections.
- [Issue #3: Uninitialized append for contiguous containers](https://github.com/sg16-unicode/sg16/issues/3)
  - Mark noted that [P1010](http://wg21.link/p1010) was not presented in Rapperswil; hopefully
    it will be in San Diego.
- [Issue #4: basic_string specification cleanup ](https://github.com/sg16-unicode/sg16/issues/4)
  - Mark mentioned that Tim Song recently proposed some clanup, but those changes don't address
    Mark's iterator invalidation concerns.
- [Issue #5: char8_t (WG21 P0482, WG14 N2231)](https://github.com/sg16-unicode/sg16/issues/5)
  - Tom stated that this is on target for C++20.  Tom has some minor wording changes to make per
    request from early LWG review.
  - Mark asked about the WG14 proposal.
  - Tom replied that WG14 is meeting again in October and that he hopes to have a revision
    ready to present.
- [Issue #6: Specify that char16_t and char32_t literals are UTF-16 and UTF-32 respectively](https://github.com/sg16-unicode/sg16/issues/6)
  - Tom indicated that the paper for this issue, [P1041R1](http://wg21.link/p1041r1), is ready
    for presentation in San Diego.
- [Issue #7: Modern terminology updates](https://github.com/sg16-unicode/sg16/issues/7)
  - Zach observed that this is something that could be done for C++20 since the changes won't
    impact implementors.
  - Tom agreed but lamented a lack of time for working on it now.
- [Issue #8: Explicitly disallow unnamed Unicode codepoints in http://eel.is/c++draft/lex.charset#2](https://github.com/sg16-unicode/sg16/issues/8)
  - Tom expressed a belief that this issue is complete.  Martinho discussed it with CWG members
    in Rapperswil and submitted a [pull request](https://github.com/cplusplus/draft/pull/2201)
    that was accepted as an editorial issue.
  - \[Editor's note: Tom was mistaken.  The accepted pull request addressed a terminology issue
    ("short name" vs "short identifier"); the concern tracked by this issue remains, though
    Martinho has a draft paper [D1139](https://github.com/sg16-unicode/sg16/blob/master/papers/d1139r0.md)
    that addresses it.\]
- [Issue #9: Requiring wchar_t to represent all members of the execution wide character set does not match existing practice](https://github.com/sg16-unicode/sg16/issues/9)
  - Artem summarized: the standard requires that all members of the execution wide character set
    be representable in a single `wchar_t` value.
  - Zach stated a preference for treating this as low priority.  Mark agreed.
  - Zach added that `wchar_t` is already a portability nightmare and there is therefore little
    incentive to try and fix it.  Mark agreed.
- [Issue #15: Add support for named Unicode character escapes](https://github.com/sg16-unicode/sg16/issues/15)
  - Tom indicated that the paper for this issue, [P1097R1](http://wg21.link/p1097r1), is ready
    for presentation in San Diego.
- [Issue #16: code_point_sequence\[\_view\]](https://github.com/sg16-unicode/sg16/issues/16)
  - Tom mentioned that Lyberta, the individual that filed this issue, had also discussed it on the
    mailing list.
  - Zack asked for clarification regarding what this issue is about.
  - Mark summarized: this is the question of whether a `text` type should have `begin()` and
    `end()` members that iterate over grapheme clusters or code points or whether the type should
    not be a range, but provide explicit access to EGC and code point ranges.
  - Tom added that Lyberta had also wanted to expose differences between encoding schemes and
    encoding forms, though it seems this was driven by purity of design goals rather than use
    cases.  Lyberta appeared to want to be able to, effectively, reinterpret cast a sequence of
    UTF16-BE code units (bytes) to a sequence of UTF-16 code units (`char16_t`).  But that doesn't
    work (portably) because bytes and `char16_t` might be the same size.
  - Mark commented, well that is fine, but don't put that in the standard then.  That's why we like
    C++; it lets you break the rules.
- [Issue #30: Unclear behavior for octal and hex escape sequences in Unicode character and string literals](https://github.com/sg16-unicode/sg16/issues/30)
  - Tom expressed a preference for making character literals like `u8'\x80'` well-formed; this
    matches existing practice.
  - Zach disagreed and presented the perspective that `u8`, `u`, and `U` literals should always
    produce well-formed UTF sequences.
  - Tom objected with the observation that `u8'\x80'` can't produce well-formed UTF-8 since it
    only produces a single code unit.
  - Zach suggested that perhaps `u8'\x80'` should be allowed, but `u8"\x80"` should not be.
  - Mark stated that both should be allowed because the programmer explicitly used a hex (or octal)
    escape sequence.
  - Zach objected saying that if he were to use an escape sequence that he wants the compiler to
    validate it.
  - Mark admitted seeing Zach's point.
  - Zach stated that, if a programmer wants to create an ill-formed sequence for some reason, then
    they should use `bit_cast` from a `char` sequence after creating the data.  The intent of adding
    a `u8` prefix to a string is to request well-formed UTF-8.
  - Tom disagreed and stated the intent of adding a `u8` prefix is to enable transcoding from the
    source character set to UTF-8.
  - Mark noted that this distinction is important due to planned changes for `char8_t`.
  - Tom disagreed and stated this is orthogonal since it is independent of the type system.
  - Tom noted that we can address this as a core issue or by writing a paper.
  - Mark said we should write a paper since there are different options for what the behavior
    should be.  Zach agreed.
  - Tom suggested that a core issue be filed to address the difference in what the standard states
    and in what current implementations actually do.  A separate paper can then address what the
    desired behavior is.
  - Zach stated that he doesn't think a defect report suffices to address this.
  - Tom stated that he'll file a core issue; Zach and Mark can follow up with a paper.
  - Mark mentioned that Martinho has a stake in this; that he wanted hex and octal escapes to be
    a back door.
  - JeanHeyd confirmed and agreed that hex and octal escapes should function as back doors.  If
    a programmer wants to ensure well-formed UTF, use `\u` or `\U` or (hopefully soon), `\N{}`.
- [Issue #31: std::text and std::text_view](https://github.com/sg16-unicode/sg16/issues/31)
  - Tom: On-going.
- [Issue #32: std::char_traits<char16_t>::eof() requires uint_least16_t to be larger than 16 bits (LWG#2959)](https://github.com/sg16-unicode/sg16/issues/32)
  - Tom summarized: All 16-bit values are valid UTF-16 code units.  This doesn't leave any room for
    a 16-bit value to be used to indicate EOF.  Implementations often use `0xFFFF` to indicate EOF.
    The result is spurious mismatches with `std::char_traits<char16_t>::eof()` when text encodes
    (valid) UTF-16 `0xFFFF` code units.
  - Zach observed that this isn't solvable without switching to a larger `int_type`.
  - Tom agreed but noted that it is an ABI break.
  - Tom added that libstdc++ made a change to minimize problems by mapping `0xFFFF` code units
    to `0xFFFD` when comparing against `eof()`, but this doesn't solve the problem.
- Tom asked what should be on the list for C++20.  Our `char8_t`, `char16_t` and `char32_t`
  literals are UTF-16/UTF-32, named escape sequences, and uninitialized string append proposals
  are underway.  We could make progress on other issues or work towards C++23 goals like
  `std::text` and `std::text_view`.
- Zach observed that the direction group would likely prioritize feature work over existing
  issues.
- Tom agreed and summarized, it sounds like prioritize features, resolve issues
  opportunistically.
- Zach then provided an update on Boost.Text.  He expects to have it ready for submission for
  Boost review soon; David Sankel has agreed to assist.
- Zach added that he got collation based text searching working and that it was fun because he
  could use Boyer-Moore searching for it.  He asked if any of us had used full collation based
  searching before.
- Artem responded that most people want linguistic searching; for example, searches for "frog"
  return "toad".
- Mark observed that linguistic searching goes a bit beyond Unicode.
- JeanHeyd asked if we should be considering exposing the Unicode character database.  Python
  and Java do \[Editor's note: and the next version of Swift will\].
- Tom was unsure and noted that programmers need for properties like "is number" and "is space"
  often have more strict constraints than Unicode; e.g., when parsing some mini-language.
- Zach added that, for full text processing, you're generally not looking at those properties
  either.
- Mark observed that adding the timezone database nearly made some committee members oppose the
  feature due to the extra 1MB or so of size.


# July 11th, 2018

## Draft agenda:
- Discuss what we want to learn from Swift and WebKit developers.
- Potentially review papers from the Rapperswil post-meeting mailing.
- Review issues list and start identifying goals for San Diego.


## Meeting summary:
- Attendees:
  - Artem Tokmakov
  - Mark Zeren
  - Tom Honermann
  - Victor Zverovich
- Apologies to JeanHeyd Meneide and Steve Downey; It seems technical issues with BlueJeans prevented
  them (and others?) from joining the meeting.  This issue and conflict with the World Cup semi-finals
  reduced attendance.
- Tom reconfirmed intent to rename our mailing list, but has not yet made progress on doing so.
- We then started reviewing some papers from the Rapperswil post-meeting mailing.
- [P0732R2: Class Types in Non-Type Template Parameters](http://wg21.link/p0732r2)
    - Tom asked if `std::text` and/or `std::text_view` should be literal types?
    - Tom noted this would require defining `operator<=>`.
    - Mark suggested adding a `std::text_literal`, but then asked about motivation:
      - `char8_t` allows differentiating encoding for standard mandated encodings.  Is there a need
        to track encoding through non-type template parameters?
      - [P0784](http://wg21.link/p0784) would enable dynamic allocation for literal types, so a
        separate (non-allocating) type may not be required.
    - Victor asked why `operator<=>` is relevant.  
    - Tom explained that `operator<=>` is required for non-type template parameters, but defining it
      for text is problematic because it would be either expensive, or wrong for many use cases
      (e.g., because it would be code unit or code point based).
    - Tom suggested that `std::fixed_string` may suffice since `std::text_view` could be layered
      on top.
    - Mark observed a solution would still be needed for encoding tagging then.
- [P1030R1: std::filesystem::path_view](http://wg21.link/p1030r1)
  - Tom mentioned that we had reviewed the earlier P0 revision during our
    [May 30th meeting](#may-30th-2018).
  - Tom noted that this revision addresses the concern we had with the `char` based interfaces
    requiring UTF-8 encoding.  However, it addresses this by replacing the `char` based interfaces
    with `std::byte` based ones.  This doesn't match existing practice for file name interfaces.
  - Tom mentioned that he would have liked to poll on this change, but since we didn't have a
    quorum, we would not do so.  The poll would have been to restore the `char` based interfaces,
    but to match the encoding requirements for `std::filesystem::path`.
- [P1100R0: Efficient composition with DynamicBuffer](http://wg21.link/p1100r0)
  - Tom wondered if Mark wanted to look at this as potentially related to
    [P1010](http://wg21.link/p1010).
  - Mark responded that he felt it isn't strongly related.
- We then discussed Victor's recent
  [follow up email](http://www.open-std.org/pipermail/unicode/2018-July/000103.html) regarding
  [P0645](http://wg21.link/p0645) and interpretation of field widths.
  - Mark stated that this is fundamentally a console problem, but that field widths are needed
    to implement programs like Eric Niebler's range based calendar example.
  - Mark also asked if we can specify that fill characters only consume one column of output.
  - Tom asked if we can rule out grapheme clusters as the unit of field width on the basis that
    the library must support non-Unicode encodings.
  - Victor suggested we could define a encoding agnostic concept of grapheme clusters.  For
    Unicode, the concept is a 1x1 match with grapheme clusters.  For other encodings, that concept
    might map to code points with no higher abstraction.
  - Tom replied that doing so is viable and that `text_view` would have to do so if its
    `Character` concept were to be redefined in terms of grapheme clusters.
  - Victor reiterated that he wants to implement both code point and grapheme cluster based
    approached and explore use cases.
  - Tom observed that the concerns are effectively equivalent for consoles and text editors;
    assuming use of a monospaced font.
  - Tom asked if `format` is intended as a `printf` replacement.
  - Victor responded, yes, but that doesn't mean that we have to replicate prior mistakes.
  - Tom suggested an experiment: Take Eric's calendar program and modify it to display emojis
    for holidays; e.g., U+1F384 Christmas Tree on December 25th.
- Discussion then turned to questions we'd like to discuss with the Swift and WebKit teams.
  - JeanHeyd (absent due to technical problems), provided the following five questions via Slack:
    - JM1: How many bug reports are related to users incorrectly choosing which layer of abstraction
         to work with for Strings (code units / code points / grapheme clusters)?
      - Tom attempted a clarification; since Swift strings are graphme cluster based, I think this
        question means, are users trying to do things at the grapheme cluster layer when they
        would be better served working at the code unit or code point level?
      - Mark posed the correlated question, how often do users try to work at code unit or code
        point level when they should just work at the grapheme cluster level?
    - JM2: Has the decision to use Extended Grapheme Clusters presented a problem (minor or major)
         in the usage?
      - Mark stated this should be the first question we ask.
      - Mark presented a different way of asking this question: What have been the best and worst
        results of this choice?
    - JM3: Has anyone ever wanted to pry underneath the string abstraction and perform their own set
         of text processing that wasn't supported by the language (e.g., retrieve code units / code
         points so they can do something that Swift did not let them do)? If so, does it happen often?
      - Tom stated the answer to the first question is clearly yes.  The second question is more
        about how often this happens and what the use cases are that motivate doing so.
      - \[Editor's note: a use case may be to work around differences in grapheme cluster boundaries
        in different Unicode versions depending on the version of Swift or the underlying version
        of ICU.\]
      - Mark expressed an interest in string builder use cases.  How are custom string builders
        created?
    - JM4: Has Swift ever considered exposing lower-level unicode database code point / script
         properties? CharacterSet seems to have some of that functionality, but has more ever been
         requested / asked for?
      - Tom expressed enthusiasm for this question.
    - JM5: There's some indication that putting the normalization form and such in the type system may
         prove beneficial. Has there been any progress on that front? We are looking to answer a
         similar question for C++ up-front, and picking one normalization form that might have the
         most up-front processing and performance benefits for typical users.
       - Mark rephrased as, what was the rationale for choosing the current design?
  - Tom then went over a list of questions he had come up with:
    - TH1: The Swift string manifesto is about 1 1/2 years old.  What have you learned since?
    - TH2: If you were starting over, what would you change?
      - Tom stated that this isn't a very useful question; it's too open ended.
      - Mark stated that bug reports are more intersting; What have you had to change?
    - TH3: How tied is the Swift string implementation to ICU?
      - Tom stated the intent of this question is to identify how much of ICU is needed to create
        a useful Unicode string class.
      - Tom added a second goal: to determine if the Swift developers would potentially be interested
        in replacing uses of ICU with standard C++ library features, if they existed.
    - TH4: Swift's string is locale insensitive (yay!).  Was a locale sensitive one considered?  Perhaps
         as a distinct type?
      - Tom stated the intent is to explore if a distinct type for localized strings might be useful
        (since locale is a run-time property not available at compile-time).
    - TH5: How often does string interpolation suffice vs using string formatting?
      - Tom asked Victor if he had considered string interpolation support when designing his `format`
        library.
      - Victor responded, yes, but with uncertainty regarding how to do it in C++ today.  Python started
        with a formatter and added interpolation later.  We could do likewise.
    - TH6: Has canonical string equality been...
      - A performance issue?
      - A surprise to users?
    - TH7: Have substrings turned out to work as well as hoped?
      - Tom noted that Swift substrings seem superficially similar to `std::string_view`, but with
        dynamic lifetime management of the underlying storage.
    - TH8: Are the results of string interpolation always dynamic?  Does Swift have a constexpr equivalent
         and, if so, do they work there?
    - TH9: Would you remove string.count() (returns "character" count) if you could?
      - Tom posed an additional question: How often do people use string.count() incorrectly?
    - TH10: Are the unicodeScalars, utf8, and utf16 views allocating?  Or are they lazy transformations?
    - TH11: There are a variety of "unsafe" methods.  Have they been problematic?
  - Mark suggested an additional question:
    - MZ1: Swift comparisons are provided.  Do users use them incorrectly?  Have they been a performance
           problem?
- Tom stated that our next meeting will be scheduled for July 25th.


# June 20th, 2018

## Draft agenda:
- Rapperswil recap.  Progress!
- Continue review of P0645R2 (Text Formatting), hopefully with
  Victor present if he can attend.
- Review the draft D1097R0 proposal:
  - https://github.com/rmartinho/sg16/blob/master/papers/d1097r0.md
- Discuss what we want to learn from the Swift and WebKit developers.

## Meeting summary:
- Attendees:
  - Corentin Jabot
  - JeanHeyd Meneide
  - Keld Simonsen
  - Mark Zeren
  - Martinho Fernandes
  - Peter Bindels
  - Steve Downey
  - Tom Honermann
  - Victor Zverovich
  - Zach Laine
- First order of business was to ensure that papers requiring updates following the Rapperswil meeting
  are submitted in time for the post-Rapperswil mailing.  Tom confirmed that P0482R4 had been submitted
  and correspondence with Hal confirmed that P1025R1 (adopted at Rapperswil) will be included in the
  mailing.  Though not discussed in Rapperswil, Martinho plans to submit a revision of P1041 for the
  mailing.
- [P0645R2](http://wg21.link/p0645r2) - Text Formatting
  - Victor started us off with a brief introduction of recent changes and review in Rapperswil.
  - Victor reported having read the summary of our previous meeting and discussion of P0645.
  - Discussion resumed regarding what field widths mean for multibyte encodings and combining characters.
  - Victor asked if basing field widths on grapheme clusters would be appropriate.
  - Zach provided an example of family emojis.  Consider 4 person code points separated by zero width
    joiners.  Each person code point combined with a ZWJ is a distinct grapheme cluster, but a single
    glyph may be used to display all four clusters.  So, grapheme clusters are not the right abstraction
    for field width.
  - Tom claimed that `format` should be used to format code units.
  - Peter suggested assuming one column per code point.
  - Keld asked about other libraries; are there any that use abstractions above code points for field
    formatting?
  - Tom stated that the competition is `printf` and iostreams.
  - Keld asked what ICU does.
  - Zach responded that he wasn't sure, but that Python uses code points for field formatting.
  - Discussion then moved on to other topics briefly.
  - Zach expressed enthusiasm for `format_to_n`.
  - Tom asked if mixed character encodings are supported.  For example:
    - `format("{}", u"text"); // execution character encoding for format string with UTF-16 argument.`
  - Victor stated that mixed encodings are not supported and result in compilation failure.
  - Zach observed that, if `char8_t` overloads were added, that, internally, `format` must consume
    code points.
  - Tom responded that this is true for any multibyte encoding, and therefore true in general for
    the execution and wide character encodings.
  - Victor agreed, but noted that operations other than fill and field formatting could be optimized
    to avoid looking at code points.
  - Peter asked if any multibyte encodings allow a NUL byte in trailing code unit sequences.  No
    such encodings were named.
  - Peter observed that, if an encoding library is used, `format` can always just read code points.
  - Zach offered to provide Victor code using code point iterators from Boost.Text that could be used
    to prototype code point based approaches.
  - Discussion briefly turned to portability of `wchar_t` and Keld's work to increase the number of
    C interfaces that do not rely on global program state; e.g., locale data.  Keld wants to improve
    support for working with multiple encodings in a single process.
  - Tom noted that such improvements are useful for our ideas around use of compile-time known
    internal encodings with transcoding to run-time determined encodings at program borders.
  - Tom asked how `format` handles signed and unsigned char; are they treated as integral/arithmetic
    or character types?
  - Victor replied that he didn't recall and would have to check.
  - Keld asked about reentrancy.
  - Victor responded that the only global state references are for locale data.
  - Keld recommended allowing strings to be tagged with encoding data.
  - Tom tried to bring discussion back to fill operations and field widths; are we agreed on use of
    code points for field fill/alignment?
  - Martinho asked how a code point approach works when writing to a fixed width buffer (of code units).
  - Victor mentioned that `format_to_n` takes a code unit count constraint.
  - Peter observed that a code unit count constraint can result in truncated code unit sequences.
  - Victor suggested that `format_to` could produce code points instead.
  - Steve asked how to avoid writing broken code; code points produced are likely going to be written
    to a code unit buffer anyway.
  - Keld stated that programmers like to write both code unit and code point code; perahps both
    should be supported.
  - Martinho claimed that truncated code unit sequences are probably not a large concern; buffers are
    generally larger than required anyway.
  - Discussion again drifted towards encodings that are known at compile-time vs run-time.
  - Keld asked what types are generally used for double byte character sets; Japanese, Chinese, ...
  - Martinho responded that those tend to be variable length encodings that switch between single byte
    and multibyte.
  - Tom agreed and mentioned ISO-2022 and escape sequences.
  - Discussion drifted back to code units vs code points.
  - Zach suggested that programmers will expect the output encoding to match the format string, but
    that code points are more consistent and natural.  If the `n` in `format_to_n` means something
    different than for field widths, that will be a problem.
  - Victor agreed that programmers will expect to be filling a code unit based buffer.
  - Tom observed that more discussion would be useful, but that we need to move on.
  - Zach recommended trying to support both code unit and code point based approaches and observe
    feedback and usage.
- [D1097R0](https://github.com/rmartinho/sg16/blob/master/papers/d1097r0.md) - Named character escapes
  - Martinho started by requesting feeback on:
    - name matching (currently more limited than described by
      [UAX44-LM2](https://www.unicode.org/reports/tr44/#UAX44-LM2))
    - lack of support for named character sequences.
  - Tom recommended adding a small section that summarizes what is actually proposed.  At present, the
    paper presents a number of options, but one must read the proposed wording to determine which
    options are actually proposed.
  - Tom expressed a preference for following the UAX44-LM2 for name matching.
  - Martinho responded with a dislike for the `U+1180 HANGUL JUNGSEONG O-E` exception and noted that
    none of the other languages he surveyed use `UAX44-LM2` for matching.
  - Keld noted existing APIs that allow specifying precision for matching.
  - Martinho clarified that general collation APIs don't apply here (because of the
    `U+1180 HANGUL JUNGSEONG O-E` exception).
  - Tom asked if we should propose this for C and everyone responded yes.
  - Tom mentioned the paper should address the potential for code breakage.  `"\N"` has a meaning now
    (it means `"N"`).
  - Tom asked if it is permissible to construct these escapes using macro concatentation.
  - Tom observed that `'_'` seemed to be missing in the definition of `c-char`.
  - Martinho stated that is intentional; `'_'` would be needed for `UAX44-LM2` matching, but that
    actual character names never use `'_'`.
  - Zach suggested adding a Tony Table to compare use of `\U` and `\N{}` escapes.
  - Tom suggested clarifying that `\N{}` escapes would not be permitted in identifiers.
  - Tom asked about interaction with raw string literals; `r-char-sequence` doesn't seem to include
    `universal-character-name`.
  - Martinho responded that `universal-character-name` escapes are not recognized in raw string
    literals; following existing precedent.
- Rapperswil recap:
  - Tom asked if Rapperswil attendees were able to connect with authors of previously discussed
    papers in order to deliver our feedback.
  - JeanHeyd reported that connections did not happen however:
    - P1030 was not discussed in Rapperswil.
    - P0882 was discussed in LEWG but not well received.  No need for follow up.
    - P0540 was discussed; LEWG feedback matched ours, so no need to follow up.
- We ran out of time to discuss what we want to learn from the Swift and WebKit developers.
- Tom asked about renaming the SG16 mailing list from `unicode` to `sg16-unicode`.  Both Tom and
  Martinho had been annoyed by the similarity to the `unicode.org` mailing list by the same name.
  No objections were raised; Tom will follow up with Keld.
- Tom noted that our next regularly scheduled meeting would fall on July 4th, a US holiday.  The
  next meeting will be scheduled for July 11th.

  
# May 30th, 2018

## Draft agenda:
- Discuss plans and goals for those attending Rapperswil.
- Review and discuss the following papers from the Rapperswil pre-meeting mailing:
  - P1030R0: std::filesystem::path_view 
  - P0540R1: A Proposal to Add split/join of string/string_view to the Standard Library
  - P0645R2: Text Formatting
  
## Meeting summary:
- Attendees:
  - JeanHeyd Meneide
  - Mark Zeren
  - Martinho Fernandes
  - Peter Bindels
  - Sergey Zubkov
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- Administrative updates:
  - Tom reported that WG chairs were contacted regarding SG16 requests for paper reviews
    in Rapperswil.  WG chairs are predictably swamped and prioritizing as best they know
    how, but we may not get to present any of our papers.
  - Zach observed that Titus is concerned about the amount of time that LEWG will need for
    ranges, but that LWG should be more concerned.
  - Tom relayed that JF Bastien volunteered to arrange introductions with Swift and WebKit
    developers working on Unicode.  Tom reached out to arrange meetings, but hasn't heard
    back.  Apple developers are busy preparing for WWDC; Tom will reach out again soon.
  - Tom brought up the recent news that Microsoft has added beta support for UTF-8 as a
    system code page as of the Windows 10 April update.  Tom made some new contacts
    within Microsoft, but has not yet gotten any further information about Microsoft's
    goals or plans with this change.
- Rapperswil planning:
  - Tom asked for volunteers to standup for SG16 at the Saturday plenary in Rapperswil
    and give a brief update.  Martinho and JeanHeyd agreed to do so.
  - Tom asked for those who have attended meetings before to offer any advice they have
    for first time attendees.
  - Zach recommended spending some time in each of the WGs.  Each WG has its own personality;
  - It was noted that hanging around in WGs where one has a short paper in the queue creates
    opportunities to present earlier than the paper might otherwise be scheduled.  The
    P1025 (normative Unicode reference) and P1041 (char16_t/char32_t are UTF-16/UTF-32)
    papers are good candidates.
  - Zach also mentioned not to be afraid to ask questions and to try to read papers ahead of
    time.
  - Tom noted that anyone present in the room is allowed to vote in straw polls, but that
    polls in plenary are generally restricted to ISO members.  It was noted that Herb will
    make it clear when ISO membership is required to vote.
- [P1030R0](http://wg21.link/p1030r0) - std::filesystem::path_view
  - Martinho liked it, especially section 4.1 (Assume UTF-8 for char based interfaces).
  - Tom liked it with the exception of section 4.1.
  - Tom expressed a belief that the discussion in section 4.1 of how existing char based
    interfaces on Windows handle conversion to wchar_t for invocation of native filesystem
    interfaces is incorrect.  Tom's understanding is that char based strings are transcoded
    to wchar_t strings using the system code page.
  - Zach asked what is meant by ANSI encoding.
  - Tom explained that Microsoft has long referred to char based encodings collectively as
    ANSI encodings despite these encodings not reflecting an ANSI standard.
  - \[Editor's note: Microsoft's glossary of terms on MSDN describes the origin of the ANSI
    reference here.  It comes from a draft ANSI specification that was eventually standardized
    as the ISO-8859 family of encodings.  See the definition of "ANSI" at
    [https://msdn.microsoft.com/en-us/goglobal/bb964658.aspx#a](https://msdn.microsoft.com/en-us/goglobal/bb964658.aspx#a).
    Microsoft now officially refers to these encodings as "Windows code pages".\]
  - Zach initiated a discussion on compile-time vs run-time encodings.  Section 4.1 describes
    a scenario in which file paths are pasted into source code as string literals, but the
    existing interpretation of such strings, when used as paths at run-time, depends on
    run-time locale settings.
  - Peter mentioned that the Microsoft compiler now supports a `/utf-8` option that purports
    to define the source and execution character encodings.  However, that option really only
    affects how literals from the source code are translated to the execution character
    encoding (UTF-8 at compile time, but never UTF-8 at run-time (at least, not until the newly
    introduced beta support in Windows 10 that requires the user to opt in)).
  - Tom stated that we can't fix the compile-time vs run-time aspects of the execution character
    encoding.
  - Martinho countered that `char8_t` offers a solution for this - we know the compile-time
    and run-time encoding of `char8_t` characters and strings.
  - Tom suggested a response to the author: maintain consistency with existing code; `char`
    means "ANSI" encoding.  Use `char8_t` for UTF-8 (follow the changes to `path` proposed in
    the `char8_t` proposal.
  - Tom, Zach, and JeanHeyd all noted the presence of `#ifdef`s surrounding the `wchar_t` based
    interfaces in the proposed design.  We don't use `#ifdef` as specification for implementation
    defined features.
  - JeanHeyd noted that that `path_view` should not fight with the platform; don't propagate
    implementation defined behavior through interfaces to the programmer.
  - Martinho observed that there is no rationale for providing `wchar_t` based interfaces only
    for Windows; they are perfectly applicable to other platforms as well.
  - Zach stated that `path_view` should work the same as `path`; just as `string_view` does for
    `string`.  `path_view` should support the same set of constructors that `path` has and they
    should behave the same.  If there is a need for new constructors, they should be added to
    both `path` and `path_view`.
  - Zack noted that `path_view` should be explicitly constructible from `path`, not the other
    way around.  \[Editor's note: as currently specified, `path_view` is constructible from
    `path`, though the constructor isn't explicit.  Note that `string_view`'s corresponding
    constructor is also not explicit. \]
  - Further discussion regarding memory allocation and the behavior of the proposed `c_str`
    class ensued.  \[Editor's note: few details of this discussion were recorded.  From what
    I recall, consensus was that the memory allocation behavior should be implementation
    defined.\]
  - JeanHeyd asked how we should communicate our feedback to the author.
  - Zach replied with a preference for a direct person-to-person response.
  - JeanHeyd volunteered to deliver feedback.
  - Poll: Use execution character encoding for `char` interfaces, `char8_t` for UTF-8?
    - Unanimous consent.
- [P0882R0](http://wg21.link/p0882r0) - User-defined Literals for std::filesystem::path
  - Tom stated that SG16 concerns are limited to encoding issues; LEWG should address any
    other concerns; e.g., naming.
  - Peter noted that the paper punts on UTF-8 support pending a solution from the comittee for
    differentiating ordinary and UTF-8 string literals.  Fortunately, we have a solution for
    that in the works!
  - It was asked why the UDLs are not `constexpr`; the answer is because they produce `path`
    objects and the `path` constructor allocates.
  - Mark asked if the UDLs should produce `path_view` objects ala P1030 above and was rewarded
    with a round of yeses.
  - Peter observed that the UDL names are very generic (ha ha) and that the literal namespace
    proposed for them differs unnecessarily from existing precedent (e.g.,
    `std::filesystem::literals` vs `std::literals::filesystem`.  \[Editor's note: This design
    also results in the UDL declarations being visible following `using namespace std::filesystem`;
    this may be intentional.\]
  - Poll: Contingent upon adoption of `char8_t`, add `char8_t` based overloads?
    - Unanimous consent.
- [P0540R1](http://wg21.link/p0540r1) - A Proposal to Add split/join of string/string_view to the Standard Library 
  - Tom observed that the paper number and filename do not match.  \[Editor's note: Tom
    followed up with Hal and the author.\]
  - Everyone in unison, "non-member functions please!"
  - Tom asked if there were any concerns about split/join functions operating at the code unit
    level.
  - Martinho replied, no, those are useful operations for splitting/constructing grapheme
    clusters.
  - Zach expressed concern about increasing the surface area of string based interfaces.
  - Poll: Does adding these additional functions complicate future efforts due to increasing
    the set of functionality to replicate at code point or higher levels?

    |  SF |   F |   N |   A |  SA |
    | --: | --: | --: | --: | --: |
    |   5 |   1 |   1 |   0 |   0 |

- [P0645R2](http://wg21.link/p0645r2) - Text Formatting
  - Zach requested `char8_t` overloads.  \[Editor's note: Peter has been planning to work on
    adding `char16_t` and `char32_t` support.  There is an existing issue tracking support
    for `char16_t`: https://github.com/fmtlib/fmt/issues/698.  That issue notes that support
    for `std::numpunct<char16_t>` is missing; that would presumably be an issue for `char8_t`
    support as well.\]
  - Zach observed that formatting only works for trivial encodings in which one code unit
    equals one code point; otherwise, field alignments won't match up in displayed text.
  - Martinho responded that, if a font is missing a glyph for a combining character, then the
    combining character will likely be displayed as a separate glyph.  Text layout is required
    to display aligned text (e.g., depends on console, curses, etc...).
  - Tom asked how such display concerns can be addressed; `format` is not a text display tool.
  - Zach asked how field size is specified.  Code units?  Code points?  "Characters"?
  - Peter provided a link to an existing github issue concerning field size and UTF-8.
    - https://github.com/fmtlib/fmt/issues/628
  - Tom noted that we were out of time; we'll continue discussion next time and will invite
    Victor to join us.
- Tom stated out next meeting will be scheduled for three weeks from now on June 20th.  The extra
  week is to give everyone a break following Rapperswil.


# May 16th, 2018

## Draft agenda:
- Review and discuss papers in the Rapperswil pre-meeting mailing.
- Discuss plans and goals for those attending Rapperswil.

## Meeting summary:
- Attendees:
  - Bob Steagal
  - Corentin Jabot
  - Dalton Woodward
  - Florin Trofin
  - JeanHeyd Meneide
  - Mark Zeren
  - Martinho Fernandes
  - Steve Downey
  - Tom Honermann
  - Zach Laine
- It was reported on Slack that Martinho's properly formatted UTF-8 P1041R0 paper
  was served up by open-std.org either without a `CharSet` header or with a
  Latin1 setting.  Tom contacted Hal and Keld.  Further discussion yielded a plan
  to update
  [SD-7](https://isocpp.org/std/standing-documents/sd-7-mailing-procedures-and-how-to-write-papers)
  to require UTF-8 for `.md` files and to configure the open-std.org web server to
  serve them with a `CharSet=UTF-8` header.
- Zach, Bob, and JeanHeyd shared some of their experience at C++Now.
- We then went on to review papers from the pre-Rapperswil mailing.
- [P1041R0](http://wg21.link/p1041R0) - Make char16_t/char32_t string literals be UTF-16/32
  - Tom noted a typo in the proposed wording changes for lex.ccon/4; a use of `UTF-8`
    where `UTF-16` was intended.
  - Given the encoding issues and lack of Markdown rendering support built into browsers,
    it was suggested that future papers, at least for now, be submitted in a pre-rendered
    format.
  - Martinho asked about getting the paper scheduled for discussion in Rapperswil.  Tom
    said he would forward SG16 polls on papers we discussed to WG chairs to communicate
    our position and request time in Rapperswil.  Tom will copy paper authors and expected
    presenters on this communication.
  - It was asked if there was any library impact.  Martinho responded no.  Tom noted having
    previously audited occurrences of `char16_t`, `char32_t`, `UTF-16`, and `UTF-32` and
    could not think of a case.
  - Zach suggested that, when presenting, it be emphasized that no implementation will need
    to make changes; that this is just standarizing existing practice.  Emphasize that there
    are no known implementations where the encoding used is not already UTF-16/UTF-32 and
    that a member of the C committee was consulted.
  - Poll: Those in favor of P1041R0?
    - Unanimous consent.
- [P1072R0](http://wg21.link/p1072r0) - Default Initialization for basic_string
  - Mark noted that P1072 is dependent on P1010 which is dependent on Richard Smith's P0593.
    This raised the question of prioritization and a request for SG16 to request that P0593
    and P1010 get time in Rapperswil so that progress can be made on P1072 in San Diego.  Tom
    agreed to make such a request; specifically to request that EWG entertain P0593 and that
    LEWG look at P1010 (and P1072 time permitting).
  - Mark went on to discuss applicability of P1072 to SG16.  Of particular concern are the
    issues caused by requiring null termination.  This is not a problem for `vector`, and
    hence not a concern expressed in P1010.
  - Mark pointed out that the design is used in real world code today.
  - Zach asked why `reserve()` doesn't suffice.  Mark explained the examples in the paper;
    that we currently either have to repeatedly update the size of the container with each
    addition, or eagerly resize the container and pay for an unused initialization.  The
    goal of the paper is to avoid both costs by enabling writing to excess capacity
    independently of updates to the container size.
  - Tom asked if option A is viable.  The concern is that const member functions must be
    thread safe.  A call to `resize_uninitialized()` makes uninitialized data available to
    const member functions.  Further, there is no event to indicate when the uninitialized
    data has been read and therefore no memory barier to function as a synchronization
    point.
  - Mark acknowledged that a two-phase commit approach is necessary to avoid UB.
  - Martinho observed that two-phase commit is not sufficient by itself because `basic_string`
    uses excess capacity to store a null terminator for the string; this is what allows
    the `data()` and `c_str()` member functions to be `const` qualified.  Overwriting the
    null terminator will cause UB for concurrently executing threads.
  - Mark advised SG16 to consider the consequences of providing implicit null-termination
    for string-like containers in the future.  An alternative approach would use string
    builders that append a null-terminator when they are collapsed.
  - Mark noted that the two-phase commit approach does at least allow the implementation to
    re-establish invariants (such as ensuring a null-terminator is present at the start of
    excess capacity following `insert_from_capacity()`.
  - Tom suggested an emplace-like solution might be preferred to enable preserving invariants.
  - Mark acknoledged a call-back/functor based solution would work (though it still doesn't
    address the over-written null-terminator issue).
  - Dalton asked whether making vector/string node-based containers such that data could be
    written to a new buffer and then swapped in.  This has the disadvantage of requiring that
    the current buffer be copied prior to performing the append.
  - Tom asked if any performance numbers were available.  What is the expected gain?
  - Mark responded that numbers are not available, but that Google has measured and claims the
    improvements make this feature worthwhile.  Estimate is a few percent improvement.
  - Corentin asked why not to use `vector` instead of `string`.
  - Mark responded that `string` is a vocabulary type.
  - Poll: Do we agree P1072R0 addresses a problem worth solving?
    - Unanimous consent.
  - Poll: Do we prefer option A, option B, or some option C?
    - A: 0
    - B: 2
    - C: 5
  - Mark clarified that option C, as discussed today, would be one of:
    - An emplace-like call with a call-back/functor.
    - A node-based swap.
  - Discussion moved into allocator interaction with node types.
  - Zach stated that swap is broken for PMR allocators.
  - Steve agreed and provided an elaboration; that the swap of the allocators doesn't swap
    the actual buffers.
  - Mark noted that moving a buffer between `vector` and `string` encounters complexities due
    to null-termination requirements.
  - Martinho asked how a small buffer optimized string is moved into a node type.
  - Dalton responded that you allocate.
  - Tom added, or the node type implements the SBO itself.
  - Mark expressed concern that an emplace-like call-back/functor approach may not work for
    the network use case of wanting to read data off the network directly into the buffer.
  - Zach suggested that, in a string builder approach, `vector` is the string builder.
  - Corentin expressed a preference for a specific string builder type rather than `vector`.
    Essentially a vocabulary type suited to the purpose.
- [P1025R0](http://wg21.link/p1025r0) - Update The Reference To The Unicode Standard
  - Steve briefly introduced the change as similar to what had been proposed, but not
    completed, for C++17.
  - Tom asked, why update the normative reference to specify each of Unicode 10, Unicode
    without a version indicator, and ISO 10646?
  - Steve answered, we need ISO 10646 for existing references; for example, the
    `__STDC_ISO_10646__` macro.  We want to reference the Unicode standard (in addition to
    ISO 10646) for stability guarantees and additional features.  We want to reference
    Unicode 10 to establish a minimum requirement, and the unversioned Unicode standard to
    enable implementors to adopt a newer version.
  - Tom suggested adding a non-normative note that implementors are allowed to use Unicode
    10 or newer; though they must use a corresponding version of ISO 10646.
  - Martinho stated that we need to make it clear that implementors must choose a specific
    Unicode release.
  - Tom asked if we should require a predefined macro that indicates the Unicode version.
  - Steve and Martinho both answered, maybe, but not yet as we don't actually depend on
    anything Unicode version dependent yet.
  - Poll: Those in favor of P1025R0:
    - Unanimous consent.
- Our next meeting will be May 30th; the week before Rapperswil.
- There is a WG21 administrative teleconference May 25th.
  - Tom will dial-in to give an update on SG16.  Martinho and JeanHeyd are encouraged to
    attend as well since they have papers to present.
- Those planning to attend Rapperswil:
  - Martinho, Corentin, Peter, JeanHeyd.
- Following the meeting, Martinho volunteered to present P1025R0 at Rapperswil since Steve
  will not be present.  Steve agreed.

## Assignments:
- Tom: Forward SG16 poll results to WG chairs and request time in Rapperswil.
- JeanHeyd: Research layering, concepts, required expressions, etc...
- Mark: Work on the basic_string specification cleanup paper.
- Tom and Zach: Work on a terminology update paper.


# April 25th, 2018

## Draft agenda:
- Review and discuss any draft papers targeting the Rapperswil pre-meeting mailing.
- Discuss plans and goals for those attending Rapperswil.

## Meeting summary:
- Attendees:
  - Tom Honermann
  - Zach Laine
  - Martinho Fernandes
  - JeanHeyd Meneide
  - Dalton Woodard
  - Sergey Zubkov
  - Steve Downey
  - Peter Bindels
  - Bob Steagall
- We started off with introductions from first time attendees Dalton and Sergey.
- Tom provided a few administrative updates:
  - A new sg16 repo was created under the sg16-unicode github org; the old std-text-wg
    repository is now retired.
    - https://github.com/sg16-unicode/sg16
  - A new sg16-papers repo was _not_ created under the assumption that it would be simpler
    to just use the new sg16 repository.
  - The old std-text-wg mailing list was removed; old emails were forwarded to the new
    mailing list.
  - The github projects that Tom was experimenting with have been removed in favor of
    tracking via github issues on the sg16 repository.
  - Alisdair provided some char8_t wording review; Mark is off the hook for doing so.
- We then did a round of status updates.
  - Steve provided a brief status update for the project to update normative
    references to ISO/IEC 10646 (https://github.com/sg16-unicode/sg16/issues/1).
    - Further discussion was postponed until after status updates.
  - Martinho provided a brief status update for the project to mandate that `char16_t`
    and `char32_t` literals use UTF-16 and UTF-32 encodings respectively
    (https://github.com/sg16-unicode/sg16/issues/6).
    - Further discussion was postponed until after status updates.
  - Zach provided an update on Boost text and reported having fun adding support for the
    Unicode bidirectional algorithm.
  - Jeanheyd reported that he is working on benchmarks and comparisons of Ogonek, Boost
    text, text_view, ICU, and other libraries.
  - We briefly discussed some of the work that Bob Steagal has been doing.  Bob had
    previously shared some UTF-8 conversion performance numbers with Zach and Tom.
    Bob had not yet joined the meeting, so Zach gave a brief overview.  When Bob later
    joined in, we discussed further (see below).
- We then returned to discussion on normative updates for Unicode standard references:
  - A productive discussion on Slack earlier in the day was helpful in setting
    direction.  At issue was how to handle UCS-2 and UCS-4 references in the
    standard since definitions of those terms would be lost by updating the
    normative reference to a recent standard.  It was confirmed that UCS-2 and
    UCS-4 are only referred to by the deprecated `codecvt` facets in annex D.
  - Tom had suggested in the Slack discussion that we could remove the deprecated
    features in C++20.  This would remove the existing references and avoid the need
    to retain any definitions for UCS-2 and UCS-4.
  - Zach noted that the standard practice for deprecation is to deprecate first and
    replace/remove in a future standard.
  - Steve noted that Debian code search reveals uses of the deprecated codecvt facets.
  - Martinho suggested the deprecated facets could be specified to use UTF-16/UTF-32
    instead of UCS-2/UCS-4.  This would be a technical break, but perhaps not a
    significant concern.
  - Tom noted that this would definitely break `codecvt_utf16` since it exists solely
    to convert between UTF-16 and UCS-2/UCS-4.
  - Steve stated that we can retain the old ISO/IEC 10646 reference for the deprecated
    UCS-2 and UTC-4 references, and use an updated reference for everything else.
  - Steve noted that LWG thought they had previously addressed the UCS-2/UCS-4 issue
    by deprecating existing uses, but since references still remain, the issue is not
    really addressed.
  - Tom asked how we should move forward.  With one paper addressing both the normative
    update and the UCS-2/UCS-4 references?  Two papers?  Perhaps three?
  - Zach expressed a preference to address separate concerns separately.
  - Tom expressed a preference for one paper with wording to address both issues.  The
    intent being that, if that approach were to fail, we could fall back to separating
    out the issues.  This preference is intended to avoid dependencies between the two
    issues; to avoid the case where we end up updating the normative reference, but not
    removing the UCS-2 and UCS-4 references, thus leaving undefined terms in the standard.
  - Sergey asked about the possibility of updating the deprecated facets to specify that
    the encoding conversion is implementation defined.
  - Tom: Someone (not sure who) had previously (not in this meeting) noted that there
    was already some implementation divergence regarding whether the codecvt facets
    actually do convert between UCS-2/UCS-4 vs UTF-16/UTF-32.
  - Martinho noted that the difference is unlikely to matter in real world usage because
    lone surrogates are unlikely to be present.
  - Peter countered that lone surrogates may appear in file names (on Windows where file
    names have 16-bit code units).
  - Zach stated that Windows no longer allows file names that are ill-formed UTF-16.
  - Tom noted he had heard this, but hasn't seen it confirmed.
  - Peter noted that WTF-8 exists to handle UCS-2 and malformed UTF-16.
  - Martinho stated that there is a difference between ill-formed UTF-16 and text that
    is not actually UTF-16.
  - We confirmed Steve had the guidance he felt he needed from the group to proceed
    as he deemed fit.
  - Steve stated that he will circulate papers when ready.
- We then returned to discussion on the character encoding for char16_t and char32_t
  literals.
  - Martinho have a quick overview of his draft:
    - https://github.com/rmartinho/sg16/blob/master/proposals/p0000r0_utf16_32_literals.md
  - Tom reported reaching out to the author of WG14 N2245
    (http://www.open-std.org/jtc1/sc22/wg14/www/docs/n2245.htm).  That author confirmed no
    known C compiler implementations that don't use UTF-16/32.
  - Zach said the paper looks good and asked what happens with \u escapes in ordinary
    string literals today.
  - Tom stated that is implementation defined.
  - Zach and Martinho noted that there are potentially different results for escapes vs
    conversion from source encoding.
  - Tom suggested updating the character literal wording to use consistent naming with
    the updated string literal wording.
  - Tom also noted there would be a merge conflict with the char8_t proposal.
  - Martinho noted that he had also observed that.  He also observed that the term,
    "UTF-8 character literal" is defined but never referenced.  Martinho confirmed he
    will define character/string literals using char16_t/char32_t and UTF-16/32 terms to
    match the UTF-8 related definitions.
- JeanHeyd asked about where to create new repositories for new projects.
  - Zach and Steve both suggested creating personal repositories; we can transfer
    ownership later if/when it becomes appropriate.
- Steve observed that it would be helpful to use a consistent license for the
  works we produce.
  - JeanHeyd asked what license is preferred, CC0, Boost, MIT?
  - Zach stated a preference for avoiding CC0 because of legal complexities with
    "public domain".
  - Tom expressed no particular preference but observed that CC0 is more complicated
    from a wording perspective.
  - Peter stated we should use a license that is friendly to implementors.
  - Zach responded that fear of patent bombing prevents usage in some cases.
  - We settled on using the Boost license for group projects.
- Bob then provided an introduction and shared more specifics of his work.
  - He has been working on UTF-8 conversion performance improvements and shared some
    results that he will be presenting at C++Now.
  - Peter asked if more benchmarks could be added to Bob's tests.
  - Bob answered yes and noted that Zach did the work to integrate Boost text.
- Steve reminded everyone to request paper numbers for the pre-Rapperswil meeting now.
- We confirmed that the next meeting will by held on May 16th; three weeks from now
  so as not to conflict with C++Now.

## Assignments:
- JeanHeyd: Research layering, concepts, required expressions, etc...
- Mark: Work on the basic_string specification cleanup paper.
- Mark: Complete and submit on the uninitialized append for contiguous containers paper.
- Martinho: Complete and submit the UTF-16/UTF-32 string literals paper.
- Steve: Complete and submit the paper to update ISO/IEC 10646 normative references.
- Tom: Work with Zach on a terminology update paper.
- Zach: Work with Tom on a terminology update paper.


# April 11th, 2018

## Draft agenda:
- Identify champions to research and author papers for projects discussed in the last meeting:
  - Terminology modernization
  - Mandate char16_t and char32_t string literals be UTF-16/UTF-32.
  - Identify existing features to deprecate/replace.
  - basic_string specification cleanup.
  - uninitialized append for contiguous containers.
- Determine how to contribute to and collaborate on a common code repository; building from the ground up.

## Meeting summary:
- Attendees:
  - Tom Honermann
  - Steve Downey
  - Mark Zeren
  - Zach Laine
  - JeanHeyd Meneide
  - Nicole Mazzuca
  - Florin Trofin
- We first discussed some administrative updates:
  - We now have a mailing list at http://www.open-std.org/mailman/listinfo/unicode!  Thanks to
    JeanHeyd for an initial post summarizing our current projects and goals.
  - Our Slack channel has been renamed to reflect our new SG16 branding.  Thanks to Mark for
    taking care of that.
  - We have a new SG16 GitHub organization at https://github.com/sg16-unicode.  The old
    std-text-wg repo will be retired (but preserved).  Meeting summaries will now be tracked in
    a new `sg16-meetings` repository.  We'll create additional repositories as needed.
- We then spent a little time discussing how best to make use of the new GitHub org.
  - Tom created GitHub projects for the initiatives discussed in our last meeting and asked
    for feedback regarding tracking things that way.  None of us have much familiarity with
    GitHub projects, so there wasn't much feedback.  Some experimenting will be required.  Tom
    noted that task ownership seems to depend on creating issues associated with a git repo.
  - Nicole noted that, because we now have a central org, we can create numerous repos.
  - Steve mentioned that having a repository just for papers is very useful.
  - Tom suggested creating a "sg16" repo with a Readme.md that provides an overview of
    SG16 and other introductory text; for example, links to the mailing list and Slack.
    Basically, a repo to host the Readme.md file from the old `std-text-wg` repo.  We might
    also use this repo for general management purposes; e.g., to host GitHub issues used for
    project tracking.
  - Zach asked about where to host our mythical use cases doc and suggested an approach in
    which the use cases are stored as code rather than as documentation.  This could support
    testing the use cases with various implementations (for correctness, performance,
    expressiveness, etc...).  Should we decide to submit a use cases paper, the paper could
    presumably pull directly from the code.
  - It was mentioned that being able to share code samples on godbolt.org (or another Compiler
    Explorer host) would be helpful.  Nicole suggested we reach out to Matt Godbolt to see if
    he would be amenable to adding Unicode related projects such as `ICU`.  Matt has fulfilled
    similar requests in the past; e.g., for `range-v3`.
- We then moved on to identifying champions to research and author papers for projects discussed
  in the last meeting.  
  - Steve volunteered to work on updating normative references to ISO/IEC 10646 to a revision
    that isn't older than some committee members.
  - Tom and Zack agreed to work on terminology modernization.
  - Martinho wasn't present for the meeting, but had previously indicated interest in working on
    a paper to mandate that char16_t and char32_t string literals be UTF-16/UTF-32; he reconfirmed
    this on Slack after the meeting.  Tom volunteered to help try and identify any existing
    compilers that don't use UTF-16/UTF-32.
  - Mark reaffirmed his intent to work on basic_string specification cleanup.
  - Mark also reaffirmed his intent to work on uninitialized append for contiguous containers.
  - Noone volunteered to champion work on identifying existing features to deprecate/replace.
- We briefly discussed deprecation approaches using `std::toupper()` as an example.  It was
  acknowledged that eventual removal of this function may not be feasible due to widespread use.
  Likewise, automatic refactoring may not be feasible since a suitable replacement function
  might require multiple code points (either for context or for mapping).  Nicole noted that this
  is an example of a function that can remain useful, but has a name that does not communicate
  its limitations.  Deprecating it in favor of an appropriately named alternative (e.g.,
  `ascii_toupper()`) would support automatic refactoring.
- Our next discussion focused on further collaboration.
  - Tom again expressed a desire to bring our collective efforts together to enable collaborating
    on a reference implementation of features we'd like to see in a future TS.  Perhaps taking a
    bottom up approach to building it.
  - Zach noted that his `text` implementation and documentation are designed around three distinct
    layers: strings, Unicode, and text.
  - JeanHeyd expressed an interest in surveying `Ogonek`, `text`, `text_view`, etc... to identify
    overlap and further define layering opportunities.
- Tom then asked about recent reflector discussions regarding `string_view` and what we might
  learn from them that would be applicable to view/reference types we might design.
  - Zach noted a few guidelines:
    - Don't support default comparisons between different kinds of views (e.g., string_view and
      rope_view).
    - Don't provide operators that hide complexity.  For example, an operator+ for text/string
      views that would require allocation might be surprising.
    - Owning vs non-owning is more imprtant than shallow vs deep compare.
  - Mark mentioned that string_view cannot support intrinsic optionality because a null pointer
    state is observeable (via `data()`).
- We briefly discussed O(1) complexity requirements on `begin()`.
  - Tom mentioned that his `itext_iterator` types do not currently meet this requirement because,
    given an ill-formed initial code unit sequence, `begin()` may consume an unbounded number of
    code units.  The consumption is necessary to ensure that, in the case where the end of the
    underlying code unit sequence is reached before any code points are successfully decoded,
    that `begin() == end()` will hold.
  - Zach noted that his iterators don't encounter this situation because the `text` type ensures
    that the underlying code unit sequence is always valid.  His transcoding iterators also do
    not hit this because they produce a replacement character for each ill-formed sequence
    (presumably limited to a bound length).  This would be an issue for support of an error
    policy that simply skips ill-formed code unit sequences though.
- Florin requested that we make sure to keep IBM users in mind and that we work with people from
  IBM to ensure that what we propose won't be problematic for non-ASCII based systems.  Tom
  fervently agreed and indicated he has maintained contact with Hubert Tong.
- We finished by agreeing to two meetings to be held before the pre-meeting mailing deadline
  for Rapperswil.  The deadline is May 7th; we will meet ~April 18th and May 2nd~ [Post-meeting
  writeup correction: we will meet just once on April 25th].

## Assignments:
- JeanHeyd: Research layering, concepts, required expressions, etc...
- Mark: Review char8_t for LWG wording updates.
- Mark: Work on the basic_string specification cleanup paper.
- Mark: Work on the uninitialized append for contiguous containers paper.
- Martinho: Work on the UTF-16/UTF-32 string literals paper.
- Steve: Work on the paper to update ISO/IEC 10646 normative references.
- Tom: Forward emails from the old std-text-wg mailing list to the new one and retire it.
- Tom: Create new sg16 and sg16-papers repos and retire the old std-text-wg one.
- Tom: Work with Zach on a terminology update paper.
- Zach: Work with Tom on a terminology update paper.


# March 28th, 2018

## Draft agenda:
- Jacksonville recap.
- Divide and conquer: can we focus efforts on different support areas?
  - Views, ranges, code units vs code points vs EGCs.
  - Encoding, decoding, normalization, segmentation.
  - UCD and CLDR interfaces.
  - Localization, collation, case mapping.

## Meeting summary:
- Attendees:
  - Tom Honermann
  - Martinho Fernandes
  - JeanHeyd Meneide
  - Mark Zeren
  - Peter Bindels
  - Corentin Jabot
  - Nicole Mazzuca
  - Michael Spencer
  - Zach Laine
  - Florin Trofin
- This was our first official teleconference meeting as SG16.  We discussed
  and agreed on a number of steps to take as part of the transition from the
  informal std-text-wg group:
  - The recently added std-text-wg@googlegroups.com mailing list will be removed
    in favor of an isocpp sponsored mailing list on open-std.org once it is
    created.
  - Tom confirmed with Vinnie Falco that the C++ Alliance is looking into acquiring
    a paid Slack plan.  We'll continue using Slack for now, but will rename the
    existing "std-text-wg" channel to "sg16-unicode" or, if we can't rename it,
    migrate to a new channel with the new name.
  - We'll rename the existing std-text-wg github repo to "sg16-unicode".  After the
    meeting, JeanHeyd suggested that we should migrate to a repo not tied to a
    a personal account and Tom agreed to do so.
- We then went on to discuss future plans.
- Mark suggested that we plan to get together with Apple Unicode developers before,
  during, or after the San Diego meeting.  Mark previously worked with a number of
  Apple developers that work on Unicode and they have considerable expertise that we
  would love to have available.
- We then discussed ideas we could pursue for C++20:
  - Mark expressed interest in cleanup of the basic_string specification.  For example:
    - Fix long standing issues that were not fixed in C++11.
    - Cleanup iterator invalidation rules to match existing implementations.
  - Mark also expressed interest in adding an uninitialized append capability to
    vector and basic_string.
  - Mark suggested that we start researching whether and how the ISO standard can
    reference the Unicode standard.  Questions include:
    - How do we handle the different cadence of ISO C++ releases and Unicode releases?
    - Do we allow implementors to choose a Unicode version?
    - Would it suffice to reference other ISO/IEC standards such as ISO/IEC 10646 and
      ISO/IEC 14651 that reflect portions of the Unicode standard?
      - https://www.iso.org/standard/56921.html
      - https://www.iso.org/standard/68309.html
  - Tom remarked that ISO C and C++ do not specify the encoding of u"" and U"" string
    literals, but suspects that all current compilers encode them as UTF-16 and UTF-32
    respectively.  If this can be confirmed, then we could propose mandating UTF-16 and
    UTF-32 respectively to WG14 and WG21.
  - Tom expressed a desire to modernize the terminology used in the C++ standard.  In
    particular, replacing or clarifying uses of "character" and "character set" with
    more modern terms like "code unit", "code point" and "character encoding".  This
    will be necessary for future specification and gives us an opportunity to educate
    the committee on some of these distinctions.
  - Tom also suggested that we could review the standard for interfaces that are too
    broken to be fixed and deprecate them.  For example, `std::ctype::toupper()`.
    - Nicole suggested that, for `std::ctype::toupper()` in particular, we could propose
      new interfaces that provide the same behavior, but under names that make the
      limitations clear.  For example, `ascii_toupper()` or `basic_toupper()`.
  - Tom discussed the difficulties we face today in drafting wording updates and that,
    ideally, we would have some kind of tool that would allow us to edit a fork of the
    standard LaTex sources, and then build it such that only the sections that were
    modified would be present in the resulting document (with appropriate insert/delete
    markup).  In discussion with Richard Smith in Jacksonville, Richard noted that he
    has wanted something like that for some time.  A considerable benefit of such an
    approach is that it would make the process of updating wording for more recent
    drafts much simpler.
- We next discussed some goals for Rapperswil:
  - Tom will pursue getting char8_t through CWG and LWG; likely via a delegate.
  - Topics for SG16 to discuss in session:
    - What is our long term vision?
    - What might a TS contain?
  - Work group activities:
    - Identify use cases.  We've been talking about getting a solid list of common
      text manipulation use cases together for approximately forever now.  Perhaps we
      could devote some time to doing that work?
- Finally, Peter suggested that we could propose a library-in-a-week project for the
  upcoming C++Now conference, May 6th-11th.
- Tom noted that many of us have our own pet projects at the moment, many of which
  overlap considerably.  Tom has `text_view`, Zach has `text`, Martinho has
  `Ogonek`, JeanHeyd has his own `text_view`, Corentin has his own experiment with
  normalization, etc...  How can we best collaborate and execute on a shared vision
  for the standard?

## Assignments:
- Tom: Follow up on SG16 branding activities: new mailing list, github repo, and Slack
  channel rename.
- Mark: Review char8_t for LWG wording updates.
- Everyone: How do we decide to collaborate on a single vision?


# Prior std-text-wg meetings

Meetings held by the informal std-text-wg working group prior to the
formation of SG16 are available at:
- https://github.com/tahonermann/std-text-wg/blob/master/MeetingNotes.md
