
Find all files readable by specific user (e.g.: root/current user)
```
vi test.pl
```

```
#!/usr/bin/perl

use strict;

sub recurse {
  my $path = shift;
  my @files = glob "$path/{*,.*}";
  for my $file (@files) {
    if (-d $file) r{
      if ($file !~ /\/\.$/ && $file !~ /\/\.\.$/) {
        recurse($file);
	  }
    } else {
      print "$file\n" if -w $file;
    }
  }
}
print "Writable files for current user\n";
recurse($ARGV[0]);
```

Make it executable:
```
chmod 755 test.pl
```