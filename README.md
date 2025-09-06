# DNS Domain Checker

A powerful Python tool for checking domain availability across multiple Top-Level Domains (TLDs) using DNS queries and WHOIS API integration. This tool performs comprehensive domain analysis with smart checking capabilities and detailed reporting.

## Features

- **Smart Domain Checking**: Two-tier checking system (tlds1 and tlds2) for efficient domain scanning
- **Multi-threaded Processing**: Concurrent DNS queries for improved performance
- **Multiple DNS Engine Support**: Load balancing across different DNS resolvers
- **WHOIS API Integration**: Additional verification using WHOIS services
- **Comprehensive Reporting**: Detailed CSV reports with multiple analysis options
- **Error Handling**: Robust handling of DNS timeouts, NXDOMAIN, and other DNS errors
- **Network Analysis**: IP address and nameserver uniqueness detection

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/dns-domain-checker.git
cd dns-domain-checker
```

2. Install required dependencies:
```bash
pip install -r requirements.txt
```

### Required Dependencies

```
dnspython
tldextract
pandas
requests
```

## Configuration

Create a `config.py` file with the following parameters:

```python
# Operation mode: "smart_check" or "full_check"
mode = "smart_check"

# Thresholds
tlds1_threshold = 5  # Minimum TLDs found to proceed to tlds2 check
whois_taken_threshold = 10  # Minimum TLDs to trigger WHOIS verification

# Performance settings
threads = 50  # Number of concurrent threads
timeout = 5  # DNS query timeout in seconds
delay_range = [0.1, 0.5]  # Delay range between requests

# Reporting options
full_report = True  # Generate detailed report
tlds_time_analysis = True  # Generate TLD performance analysis
dns_engines_analysis = True  # Generate DNS engine performance analysis

# WHOIS API configuration
whois_api_key = "your_api_key_here"  # APILayer WHOIS API key
```

## Setup Required Files

Create a `utils.py` module with the following functions:

- `get_domains()` - Returns list of domains to check
- `get_old_domains()` - Returns list of domains to exclude
- `get_tlds1()` - Returns primary TLD list for initial checking
- `get_tlds2()` - Returns secondary TLD list for extended checking
- `get_tlds()` - Returns complete TLD list for full check mode
- `get_dns_engines()` - Returns list of DNS servers to use
- `get_whois_tlds()` - Returns TLDs supported by WHOIS API

## Usage

### Basic Usage

```bash
python main.py
```

### Operation Modes

#### Smart Check Mode
- Performs initial check with `tlds1` list
- If domain shows promise (≥ threshold TLDs found), checks additional `tlds2` TLDs
- Efficient for large-scale domain analysis

#### Full Check Mode
- Checks all TLDs in the complete list
- Comprehensive but slower analysis

## Output Files

The tool generates several CSV reports:

### 1. Main Results
- `Smart_Check_YYYYDDMM_HHMMSS.csv` or `Full_Check_YYYYDDMM_HHMMSS.csv`
- Contains: domain, TLDs count, unique IPs count

### 2. Full Report (if enabled)
- `Full_Report_[mode]_YYYYDDMM_HHMMSS.csv`
- Detailed information for each domain-TLD combination
- Includes: IP addresses, nameservers, DNS engines used, error flags

### 3. TLD Performance Analysis (if enabled)
- `TLDs_Time_Analysis_YYYYDDMM_HHMMSS.csv`
- Performance metrics for each TLD
- Response times, error rates, request counts

### 4. DNS Engine Analysis (if enabled)
- `DNS_Engines_Analysis_YYYYDDMM_HHMMSS.csv`
- Performance comparison of DNS resolvers
- Helps optimize DNS engine selection

## Features Explained

### DNS Query Types
- **A Records**: IP address resolution
- **NS Records**: Nameserver information

### Error Handling
- `NXDOMAIN`: Domain doesn't exist
- `LifetimeTimeout`: DNS query timeout
- `NoNameservers`: No nameservers available
- `NoAnswer`: No answer from DNS server
- `ValueError`: Invalid DNS response

### Uniqueness Detection
- **Network Uniqueness**: Tracks unique IP network ranges (/16)
- **Nameserver Uniqueness**: Identifies unique nameserver configurations
- Helps identify truly independent domain registrations

### WHOIS Integration
- Verifies domains that appear unregistered via DNS
- Useful for catching parked or redirect-only domains
- Supports APILayer WHOIS API

## Performance Optimization

### Threading
- Configurable thread pool for concurrent processing
- Separate thread management for DNS engine allocation

### DNS Engine Load Balancing
- Rotates through available DNS servers
- Prevents overloading single DNS resolver
- Automatic DNS engine release after queries

### Smart Retry Logic
- Automatic retry with increased timeout for failed queries
- Prevents false negatives due to temporary DNS issues

## Example Configuration Files

### domains.txt
```
example.com
test.org
mydomain.net
```

### tlds1.txt (Primary TLDs)
```
com
net
org
info
biz
```

### tlds2.txt (Secondary TLDs)
```
co
io
me
tv
cc
```

### dns_engines.txt
```
8.8.8.8,8.8.4.4
1.1.1.1,1.0.0.1
208.67.222.222,208.67.220.220
```

## API Integration

### WHOIS API Setup
1. Sign up for APILayer account
2. Get WHOIS API key
3. Add key to `config.py`
4. Configure `whois_taken_threshold`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Troubleshooting

### Common Issues

**High timeout rates**: 
- Reduce thread count
- Increase timeout values
- Check DNS engine reliability

**Memory usage**: 
- Reduce batch size
- Lower thread count
- Enable result streaming

**WHOIS API limits**:
- Monitor API usage
- Implement rate limiting
- Consider upgrading API plan

## Changelog

### v1.0.0
- Initial release
- Smart checking mode
- Multi-threaded DNS queries
- WHOIS API integration
- Comprehensive reporting

## Support

For issues and questions:
- Create an issue on GitHub
- Check existing documentation
- Review configuration examples

---

**Note**: This tool is for legitimate domain research purposes. Please respect rate limits and terms of service for DNS providers and APIs.
