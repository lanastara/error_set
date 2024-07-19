# error_set

[<img alt="github" src="https://img.shields.io/badge/github-mcmah309/error_set-8da0cb?style=for-the-badge&labelColor=555555&logo=github" height="20">](https://github.com/mcmah309/error_set)
[<img alt="crates.io" src="https://img.shields.io/crates/v/error_set.svg?style=for-the-badge&color=fc8d62&logo=rust" height="20">](https://crates.io/crates/error_set)
[<img alt="docs.rs" src="https://img.shields.io/badge/docs.rs-error_set-66c2a5?style=for-the-badge&labelColor=555555&logo=docs.rs" height="20">](https://docs.rs/error_set)
[<img alt="build status" src="https://img.shields.io/github/actions/workflow/status/mcmah309/error_set/ci.yml?branch=master&style=for-the-badge" height="20">](https://github.com/mcmah309/error_set/actions?query=branch%3Amaster)


A concise way to define errors and ergonomically coerce a subset into a superset with with just `.into()` or `?`.
Managing errors is no longer a tedious process.

`error_set` was inspired by zig's [error set](https://ziglang.org/documentation/master/#Error-Set-Type)
and works functionally the same.

Instead of defining various enums/structs for errors and hand rolling relations, use an error set:
```rust
use error_set::error_set;

error_set! {
    MediaError = {
        IoError(std::io::Error)
    } || BookParsingError || DownloadError || ParseUploadError;
    BookParsingError = {
        MissingBookDescription,
        CouldNotReadBook(std::io::Error),
    } || BookSectionParsingError;
    BookSectionParsingError = {
        MissingName,
        NoContents,
    };
    DownloadError = {
        InvalidUrl,
        CouldNotSaveBook(std::io::Error),
    };
    ParseUploadError = {
        MaximumUploadSizeReached,
        TimedOut,
        AuthenticationFailed,
    };
}
```
<details>

  <summary>Cargo Expand</summary>

```rust
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
pub enum MediaError {
    IoError(std::io::Error),
    MissingBookDescription,
    MissingName,
    NoContents,
    InvalidUrl,
    MaximumUploadSizeReached,
    TimedOut,
    AuthenticationFailed,
}
#[automatically_derived]
impl ::core::fmt::Debug for MediaError {
    #[inline]
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        match self {
            MediaError::IoError(__self_0) => {
                ::core::fmt::Formatter::debug_tuple_field1_finish(
                    f,
                    "IoError",
                    &__self_0,
                )
            }
            MediaError::MissingBookDescription => {
                ::core::fmt::Formatter::write_str(f, "MissingBookDescription")
            }
            MediaError::MissingName => {
                ::core::fmt::Formatter::write_str(f, "MissingName")
            }
            MediaError::NoContents => ::core::fmt::Formatter::write_str(f, "NoContents"),
            MediaError::InvalidUrl => ::core::fmt::Formatter::write_str(f, "InvalidUrl"),
            MediaError::MaximumUploadSizeReached => {
                ::core::fmt::Formatter::write_str(f, "MaximumUploadSizeReached")
            }
            MediaError::TimedOut => ::core::fmt::Formatter::write_str(f, "TimedOut"),
            MediaError::AuthenticationFailed => {
                ::core::fmt::Formatter::write_str(f, "AuthenticationFailed")
            }
        }
    }
}
impl error_set::ErrorSetMarker for MediaError {}
#[allow(unused_qualifications)]
impl std::error::Error for MediaError {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        match *self {
            MediaError::IoError(ref source) => source.source(),
            #[allow(unreachable_patterns)]
            _ => None,
        }
    }
}
impl core::fmt::Display for MediaError {
    #[inline]
    fn fmt(&self, f: &mut core::fmt::Formatter) -> core::fmt::Result {
        let variant_name = match *self {
            MediaError::IoError(_) => "MediaError::IoError",
            MediaError::MissingBookDescription => "MediaError::MissingBookDescription",
            MediaError::MissingName => "MediaError::MissingName",
            MediaError::NoContents => "MediaError::NoContents",
            MediaError::InvalidUrl => "MediaError::InvalidUrl",
            MediaError::MaximumUploadSizeReached => {
                "MediaError::MaximumUploadSizeReached"
            }
            MediaError::TimedOut => "MediaError::TimedOut",
            MediaError::AuthenticationFailed => "MediaError::AuthenticationFailed",
        };
        f.write_fmt(format_args!("{0}", variant_name))
    }
}
impl From<BookParsingError> for MediaError {
    fn from(error: BookParsingError) -> Self {
        match error {
            BookParsingError::MissingBookDescription => {
                MediaError::MissingBookDescription
            }
            BookParsingError::CouldNotReadBook(source) => MediaError::IoError(source),
            BookParsingError::MissingName => MediaError::MissingName,
            BookParsingError::NoContents => MediaError::NoContents,
        }
    }
}
impl From<BookSectionParsingError> for MediaError {
    fn from(error: BookSectionParsingError) -> Self {
        match error {
            BookSectionParsingError::MissingName => MediaError::MissingName,
            BookSectionParsingError::NoContents => MediaError::NoContents,
        }
    }
}
impl From<DownloadError> for MediaError {
    fn from(error: DownloadError) -> Self {
        match error {
            DownloadError::InvalidUrl => MediaError::InvalidUrl,
            DownloadError::CouldNotSaveBook(source) => MediaError::IoError(source),
        }
    }
}
impl From<ParseUploadError> for MediaError {
    fn from(error: ParseUploadError) -> Self {
        match error {
            ParseUploadError::MaximumUploadSizeReached => {
                MediaError::MaximumUploadSizeReached
            }
            ParseUploadError::TimedOut => MediaError::TimedOut,
            ParseUploadError::AuthenticationFailed => MediaError::AuthenticationFailed,
        }
    }
}
impl From<std::io::Error> for MediaError {
    fn from(error: std::io::Error) -> Self {
        MediaError::IoError(error)
    }
}
pub enum BookParsingError {
    MissingBookDescription,
    CouldNotReadBook(std::io::Error),
    MissingName,
    NoContents,
}
#[automatically_derived]
impl ::core::fmt::Debug for BookParsingError {
    #[inline]
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        match self {
            BookParsingError::MissingBookDescription => {
                ::core::fmt::Formatter::write_str(f, "MissingBookDescription")
            }
            BookParsingError::CouldNotReadBook(__self_0) => {
                ::core::fmt::Formatter::debug_tuple_field1_finish(
                    f,
                    "CouldNotReadBook",
                    &__self_0,
                )
            }
            BookParsingError::MissingName => {
                ::core::fmt::Formatter::write_str(f, "MissingName")
            }
            BookParsingError::NoContents => {
                ::core::fmt::Formatter::write_str(f, "NoContents")
            }
        }
    }
}
impl error_set::ErrorSetMarker for BookParsingError {}
#[allow(unused_qualifications)]
impl std::error::Error for BookParsingError {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        match *self {
            BookParsingError::CouldNotReadBook(ref source) => source.source(),
            #[allow(unreachable_patterns)]
            _ => None,
        }
    }
}
impl core::fmt::Display for BookParsingError {
    #[inline]
    fn fmt(&self, f: &mut core::fmt::Formatter) -> core::fmt::Result {
        let variant_name = match *self {
            BookParsingError::MissingBookDescription => {
                "BookParsingError::MissingBookDescription"
            }
            BookParsingError::CouldNotReadBook(_) => "BookParsingError::CouldNotReadBook",
            BookParsingError::MissingName => "BookParsingError::MissingName",
            BookParsingError::NoContents => "BookParsingError::NoContents",
        };
        f.write_fmt(format_args!("{0}", variant_name))
    }
}
impl From<BookSectionParsingError> for BookParsingError {
    fn from(error: BookSectionParsingError) -> Self {
        match error {
            BookSectionParsingError::MissingName => BookParsingError::MissingName,
            BookSectionParsingError::NoContents => BookParsingError::NoContents,
        }
    }
}
impl From<std::io::Error> for BookParsingError {
    fn from(error: std::io::Error) -> Self {
        BookParsingError::CouldNotReadBook(error)
    }
}
pub enum BookSectionParsingError {
    MissingName,
    NoContents,
}
#[automatically_derived]
impl ::core::fmt::Debug for BookSectionParsingError {
    #[inline]
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        ::core::fmt::Formatter::write_str(
            f,
            match self {
                BookSectionParsingError::MissingName => "MissingName",
                BookSectionParsingError::NoContents => "NoContents",
            },
        )
    }
}
impl error_set::ErrorSetMarker for BookSectionParsingError {}
#[allow(unused_qualifications)]
impl std::error::Error for BookSectionParsingError {}
impl core::fmt::Display for BookSectionParsingError {
    #[inline]
    fn fmt(&self, f: &mut core::fmt::Formatter) -> core::fmt::Result {
        let variant_name = match *self {
            BookSectionParsingError::MissingName => {
                "BookSectionParsingError::MissingName"
            }
            BookSectionParsingError::NoContents => "BookSectionParsingError::NoContents",
        };
        f.write_fmt(format_args!("{0}", variant_name))
    }
}
pub enum DownloadError {
    InvalidUrl,
    CouldNotSaveBook(std::io::Error),
}
#[automatically_derived]
impl ::core::fmt::Debug for DownloadError {
    #[inline]
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        match self {
            DownloadError::InvalidUrl => {
                ::core::fmt::Formatter::write_str(f, "InvalidUrl")
            }
            DownloadError::CouldNotSaveBook(__self_0) => {
                ::core::fmt::Formatter::debug_tuple_field1_finish(
                    f,
                    "CouldNotSaveBook",
                    &__self_0,
                )
            }
        }
    }
}
impl error_set::ErrorSetMarker for DownloadError {}
#[allow(unused_qualifications)]
impl std::error::Error for DownloadError {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        match *self {
            DownloadError::CouldNotSaveBook(ref source) => source.source(),
            #[allow(unreachable_patterns)]
            _ => None,
        }
    }
}
impl core::fmt::Display for DownloadError {
    #[inline]
    fn fmt(&self, f: &mut core::fmt::Formatter) -> core::fmt::Result {
        let variant_name = match *self {
            DownloadError::InvalidUrl => "DownloadError::InvalidUrl",
            DownloadError::CouldNotSaveBook(_) => "DownloadError::CouldNotSaveBook",
        };
        f.write_fmt(format_args!("{0}", variant_name))
    }
}
impl From<std::io::Error> for DownloadError {
    fn from(error: std::io::Error) -> Self {
        DownloadError::CouldNotSaveBook(error)
    }
}
pub enum ParseUploadError {
    MaximumUploadSizeReached,
    TimedOut,
    AuthenticationFailed,
}
#[automatically_derived]
impl ::core::fmt::Debug for ParseUploadError {
    #[inline]
    fn fmt(&self, f: &mut ::core::fmt::Formatter) -> ::core::fmt::Result {
        ::core::fmt::Formatter::write_str(
            f,
            match self {
                ParseUploadError::MaximumUploadSizeReached => "MaximumUploadSizeReached",
                ParseUploadError::TimedOut => "TimedOut",
                ParseUploadError::AuthenticationFailed => "AuthenticationFailed",
            },
        )
    }
}
impl error_set::ErrorSetMarker for ParseUploadError {}
#[allow(unused_qualifications)]
impl std::error::Error for ParseUploadError {}
impl core::fmt::Display for ParseUploadError {
    #[inline]
    fn fmt(&self, f: &mut core::fmt::Formatter) -> core::fmt::Result {
        let variant_name = match *self {
            ParseUploadError::MaximumUploadSizeReached => {
                "ParseUploadError::MaximumUploadSizeReached"
            }
            ParseUploadError::TimedOut => "ParseUploadError::TimedOut",
            ParseUploadError::AuthenticationFailed => {
                "ParseUploadError::AuthenticationFailed"
            }
        };
        f.write_fmt(format_args!("{0}", variant_name))
    }
}

// `coerce` macro omitted

```
</details>

which is also equivalent to writing:
```rust
error_set! {
    MediaError = {
        IoError(std::io::Error),
        MissingBookDescription,
        MissingName,
        NoContents,
        InvalidUrl,
        MaximumUploadSizeReached,
        TimedOut,
        AuthenticationFailed,
    };
    BookParsingError = {
        MissingBookDescription,
        CouldNotReadBook(std::io::Error),
        MissingName,
        NoContents,
    };
    BookSectionParsingError = {
        MissingName,
        NoContents,
    };
    DownloadError = {
        InvalidUrl,
        CouldNotSaveBook(std::io::Error),
    };
    ParseUploadError = {
        MaximumUploadSizeReached,
        TimedOut,
        AuthenticationFailed,
    };
}
```
As mentioned, any above subset can be converted into a superset with `.into()` or `?`. Error enums and error variants can also accept doc comments and attributes like `#[derive(...)]`.

### `error_set!` Examples
<details>

  <summary>Base Functionality In Action</summary>

```rust
use error_set::error_set;

error_set! {
    MediaError = {
        IoError(std::io::Error)
    } || BookParsingError || DownloadError || ParseUploadError;
    BookParsingError = {
        MissingBookDescription,
        CouldNotReadBook(std::io::Error),
    } || BookSectionParsingError;
    BookSectionParsingError = {
        MissingName,
        NoContents,
    };
    DownloadError = {
        InvalidUrl,
        CouldNotSaveBook(std::io::Error),
    };
    ParseUploadError = {
        MaximumUploadSizeReached,
        TimedOut,
        AuthenticationFailed,
    };
}

fn main() {
    let book_section_parsing_error: BookSectionParsingError = BookSectionParsingError::MissingName;
    let book_parsing_error: BookParsingError = book_section_parsing_error.into();
    assert!(matches!(book_parsing_error, BookParsingError::MissingName));
    let media_error: MediaError = book_parsing_error.into();
    assert!(matches!(media_error, MediaError::MissingName));

    let io_error = std::io::Error::new(std::io::ErrorKind::OutOfMemory, "oops out of memory");
    let result_download_error: Result<(), DownloadError> = Err(io_error).coerce(); // `.coerce()` == `.map_err(Into::into)`
    let result_media_error: Result<(), MediaError> = result_download_error.coerce(); // `.coerce()` == `.map_err(Into::into)`
    assert!(matches!(result_media_error, Err(MediaError::IoError(_))));
}
```
</details>