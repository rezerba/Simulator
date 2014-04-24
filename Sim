use strict;
use warnings;

my $csv_file =  "./nakano_data(csv).csv";
my @data = &readCsvFile ($csv_file);

use Fatal qw/ open /;

sub readCsvFile {
    open (IN, $_[0]);
    while (<IN>) {
        chomp;
        push @data, [ split(/,/) ];
    }
    close (IN);
    return @data;
}

my @data_tmp;

my ($i, $j);

for ($i = 1; $i <= 24; $i++) {
	for ($j = 1; $j <= 31; $j++) {
		$data_tmp[$j][$i] = $data[$i][$j];
	}
}
@data = @data_tmp;

my @pv;

for ($i = 1; $i <= 31; $i++) {
	for ($j = 7; $j <= 18; $j++) {
		$pv[$i] +=$data[$i][$j];
	}
}

my @battery_discharge;

for ($i = 1; $i <=31; $i++) {
	for ($j = 1; $j <=24; $j++) {
		$battery_discharge[$i] += $data[$i][$j];
		}
	$battery_discharge[$i] -= $pv[$i];
}

my @battery_charge;

for ($i = 1; $i <= $#battery_discharge; $i++) {
	$battery_charge[$i] = $battery_discharge[$i] * (100 / 95);
}

my @pv_max;

for ($i = 1; $i <=$#pv; $i++) {
	$pv_max[$i] = $pv[$i] + $battery_charge[$i];
}

my @solar_time;

for ($i = 1; $i <= $#pv_max; $i++) {
	$solar_time[$i] = $pv_max[$i] / 12;
}

my $max;

sub Max{
  my @nums = @_;
  
  my $max_num;
  for my $num (@nums) {
    if (!defined $max_num) {
      $max_num = $num;
    }
    else{
      if ($num > $max_num) {
        $max_num = $num;
      }
    }
  }
  return $max_num;
}

$max = &Max(@battery_discharge);
print "max_battery_discharge = $max\n";

$max = &Max(@pv_max);
print "max_pv_max = $max\n";

$max = &Max(@solar_time);
print "max_solar_time = $max\n";
