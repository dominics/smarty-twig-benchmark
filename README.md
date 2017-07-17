# An up-to-date Smarty versus Twig benchmark

An *opinionated* comparison benchmark of the latest versions of Smarty3 and Twig.

TL;DR: Smarty is slower than Twig, Twig is faster than Smarty. Don't trust benchmarks from years ago.

## Opinionated How?

The main differences from other benchmarks you might run into:

* Both engines are tested with auto-escaping turned **on**
  * This is because you shouldn't be relying on your template developers to manually escape variables, ever. It's asking for
    XSS trouble.
  * Doing a benchmark that _disables auto-escaping_ can hide performance problems in a part of the template engine you
    *definitely* want to be using.
* This test is being performed in 2017, which means we're not even going to bother with PHP5
  * A *lot* of things are different in PHP now, for example, objects have better performance characteristics in PHP 5.4+ (https://gist.github.com/nikic/5015323)

Most other benchmarks I've seen date from around 2011 or 2012. That's ancient in web-development terms. And unfortunately,
I've seen a few PHP coders get caught out, choose Smarty because "it's faster" (or so says one benchmark), and end up without
escaping throughout their templates.

Yes, authors should probably know to escape their output. But, equally, a single oversight shouldn't result in a vulnerability.

## Templates

The test templates are pretty simple:

* Extending one base template and overriding its blocks
* 3 blocks, with varying default content
* A single for loop, outputting elements of an array within one block

## Running Yourself

Don't take my word for it:

* `composer install`
* `php bench.php smarty` or `php bench.php twig`

## Results

With Smarty 3.1.31 and Twig 2.4.3, on PHP 7.1.7, 100000 iterations, compile time ignored, cache warmed, my machine, best time
of dozens of runs each:

Benchmark | Time Taken
--- | ---
twig | 9.7 seconds
twig_reuse | 7.6 seconds
smarty | 17.8 seconds

See the code for the difference between the twig and twig_reuse scenarios (basically: using the same Twig_TemplateWrapper instance, versus
loading the template again. I'm not sure there's an equivalent for Smarty, but I'd be happy to include one if there is.)

