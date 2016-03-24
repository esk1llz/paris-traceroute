# Backward compatibility #

For the new library-based Paris Traceroute command line tool to be successful, it needs to be backward compatible both with the old C++ version of Paris Traceroute and with classic variants of traceroute.

The following pages describe the plans for developing the new Paris Traceroute so that it is backward compatible:
  * Backward compatibility with the C++ version of Paris Traceroute
    * [Compatible input](BackwardCompatibilityCppInput.md) at the command line
    * [Compatible output](BackwardCompatibilityCppOutput.md)
  * Backward compatibility with Dmitry Butskoy's classic [Modern Traceroute for Linux](http://traceroute.sourceforge.net/)
    * [Compatible input](BackwardCompatibilityModernInput.md) at the command line
    * [Compatible output](BackwardCompatibilityModernOutput.md)
  * Backward compatibility with Van Jacobson's most classic BSD Traceroute ([download from lbl.gov](ftp://ftp.ee.lbl.gov/traceroute.tar.gz))
    * [Compatible input](BackwardCompatibilityBSDInput.md) at the command line
    * [Compatible output](BackwardCompatibilityBSDOutput.md)