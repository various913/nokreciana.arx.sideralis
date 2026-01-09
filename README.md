# nokreciana.arx.sideralis
Terms and Conditions for Tenants
import React, { useState } from 'react';
import { AlertCircle, CheckCircle2, FileText, User, Users } from 'lucide-react';

export default function TenantApplicationForm() {
  const [formData, setFormData] = useState({
    tenantName: '',
    tenantEmail: '',
    tenantPhone: '',
    tenantIdType: '',
    tenantIdNumber: '',
    guardianName: '',
    guardianEmail: '',
    guardianPhone: '',
    guardianIdType: '',
    guardianIdNumber: '',
    guardianRelation: '',
    occupancyType: 'single',
    moveInDate: '',
    agreeTerms: false,
    agreeDeposit: false,
    agreeRules: false,
    agreeGuardian: false,
    tenantSignature: '',
    guardianSignature: '',
    signatureDate: new Date().toISOString().split('T')[0]
  });

  const [submitted, setSubmitted] = useState(false);
  const [showTerms, setShowTerms] = useState(false);
  const [errors, setErrors] = useState([]);

  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };

  const validateForm = () => {
    const newErrors = [];
    
    if (!formData.tenantName) newErrors.push('Tenant name is required');
    if (!formData.tenantEmail) newErrors.push('Tenant email is required');
    if (!formData.tenantPhone) newErrors.push('Tenant phone is required');
    if (!formData.tenantIdType) newErrors.push('Tenant ID type is required');
    if (!formData.tenantIdNumber) newErrors.push('Tenant ID number is required');
    if (!formData.guardianName) newErrors.push('Guardian name is required');
    if (!formData.guardianPhone) newErrors.push('Guardian phone is required');
    if (!formData.guardianIdType) newErrors.push('Guardian ID type is required');
    if (!formData.guardianIdNumber) newErrors.push('Guardian ID number is required');
    if (!formData.guardianRelation) newErrors.push('Guardian relationship is required');
    if (!formData.moveInDate) newErrors.push('Move-in date is required');
    if (!formData.tenantSignature) newErrors.push('Tenant signature is required');
    if (!formData.guardianSignature) newErrors.push('Guardian signature is required');
    
    if (!formData.agreeTerms) newErrors.push('Must agree to terms and conditions');
    if (!formData.agreeDeposit) newErrors.push('Must agree to deposit policy');
    if (!formData.agreeRules) newErrors.push('Must agree to house rules');
    if (!formData.agreeGuardian) newErrors.push('Must confirm guardian information');
    
    return newErrors;
  };

  const handleSubmit = () => {
    const validationErrors = validateForm();
    
    if (validationErrors.length > 0) {
      setErrors(validationErrors);
      window.scrollTo({ top: 0, behavior: 'smooth' });
      return;
    }
    
    setErrors([]);
    setSubmitted(true);
    console.log('Form submitted:', formData);
  };

  const rentAmount = formData.occupancyType === 'single' ? 4000 : 6000;

  if (submitted) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 flex items-center justify-center p-4">
        <div className="bg-white rounded-lg shadow-xl p-8 max-w-2xl w-full text-center">
          <CheckCircle2 className="w-16 h-16 text-green-500 mx-auto mb-4" />
          <h2 className="text-3xl font-bold text-gray-800 mb-4">Application Submitted!</h2>
          <p className="text-gray-600 mb-6">
            Thank you for submitting your tenant application for Nokreciana Arx Sideralis.
            We will review your application and contact you shortly.
          </p>
          <div className="bg-blue-50 rounded-lg p-6 text-left mb-6">
            <h3 className="font-semibold text-lg mb-3">Application Summary:</h3>
            <div className="space-y-2 text-sm">
              <p><strong>Tenant:</strong> {formData.tenantName}</p>
              <p><strong>Email:</strong> {formData.tenantEmail}</p>
              <p><strong>Occupancy Type:</strong> {formData.occupancyType === 'single' ? 'Single' : 'Double'}</p>
              <p><strong>Monthly Rent:</strong> ₹{rentAmount}</p>
              <p><strong>Security Deposit:</strong> ₹{rentAmount}</p>
              <p><strong>Move-in Date:</strong> {formData.moveInDate}</p>
              <p><strong>Local Guardian:</strong> {formData.guardianName}</p>
            </div>
          </div>
          <p className="text-sm text-gray-500 italic">
            "RENT RIGHT. RENT BRIGHT"
          </p>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 py-8 px-4">
      <div className="max-w-4xl mx-auto">
        <div className="bg-white rounded-lg shadow-xl overflow-hidden">
          <div className="bg-gradient-to-r from-blue-600 to-indigo-600 text-white p-8">
            <h1 className="text-3xl font-bold mb-2">Nokreciana Arx Sideralis</h1>
            <p className="text-blue-100">Tenant Application Form</p>
            <p className="text-sm text-blue-200 mt-2 italic">"RENT RIGHT. RENT BRIGHT"</p>
          </div>

          {errors.length > 0 && (
            <div className="bg-red-50 border-l-4 border-red-500 p-4 m-6">
              <div className="flex items-start">
                <AlertCircle className="w-5 h-5 text-red-500 mr-2 flex-shrink-0 mt-0.5" />
                <div>
                  <p className="font-semibold text-red-800 mb-2">Please correct the following errors:</p>
                  <ul className="text-sm text-red-700 space-y-1">
                    {errors.map((error, idx) => (
                      <li key={idx}>• {error}</li>
                    ))}
                  </ul>
                </div>
              </div>
            </div>
          )}

          <div className="p-8">
            <section className="mb-8">
              <div className="flex items-center gap-2 mb-4">
                <FileText className="w-5 h-5 text-blue-600" />
                <h2 className="text-xl font-semibold text-gray-800">Rental Details</h2>
              </div>
              
              <div className="grid md:grid-cols-2 gap-6">
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Occupancy Type <span className="text-red-500">*</span>
                  </label>
                  <select
                    name="occupancyType"
                    value={formData.occupancyType}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  >
                    <option value="single">Single Stay (₹4,000/month)</option>
                    <option value="double">Double Stay (₹6,000/month)</option>
                  </select>
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Expected Move-in Date <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="date"
                    name="moveInDate"
                    value={formData.moveInDate}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
              </div>
              
              <div className="mt-4 bg-blue-50 border border-blue-200 rounded-lg p-4">
                <p className="text-sm text-gray-700">
                  <strong>Monthly Rent:</strong> ₹{rentAmount}<br />
                  <strong>Security Deposit:</strong> ₹{rentAmount} (non-refundable, adjusted against last month's rent)
                </p>
              </div>
            </section>

            <section className="mb-8">
              <div className="flex items-center gap-2 mb-4">
                <User className="w-5 h-5 text-blue-600" />
                <h2 className="text-xl font-semibold text-gray-800">Tenant Information</h2>
              </div>
              
              <div className="grid md:grid-cols-2 gap-6">
                <div className="md:col-span-2">
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Full Name <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="text"
                    name="tenantName"
                    value={formData.tenantName}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Email <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="email"
                    name="tenantEmail"
                    value={formData.tenantEmail}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Phone Number <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="tel"
                    name="tenantPhone"
                    value={formData.tenantPhone}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    ID Type <span className="text-red-500">*</span>
                  </label>
                  <select
                    name="tenantIdType"
                    value={formData.tenantIdType}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  >
                    <option value="">Select ID Type</option>
                    <option value="aadhar">Aadhar Card</option>
                    <option value="pan">PAN Card</option>
                    <option value="passport">Passport</option>
                    <option value="driving">Driving License</option>
                  </select>
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    ID Number <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="text"
                    name="tenantIdNumber"
                    value={formData.tenantIdNumber}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
              </div>
            </section>

            <section className="mb-8">
              <div className="flex items-center gap-2 mb-4">
                <Users className="w-5 h-5 text-blue-600" />
                <h2 className="text-xl font-semibold text-gray-800">Local Guardian Information</h2>
              </div>
              
              <div className="grid md:grid-cols-2 gap-6">
                <div className="md:col-span-2">
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Full Name <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="text"
                    name="guardianName"
                    value={formData.guardianName}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Email
                  </label>
                  <input
                    type="email"
                    name="guardianEmail"
                    value={formData.guardianEmail}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Phone Number <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="tel"
                    name="guardianPhone"
                    value={formData.guardianPhone}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    Relationship to Tenant <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="text"
                    name="guardianRelation"
                    value={formData.guardianRelation}
                    onChange={handleChange}
                    placeholder="e.g., Parent, Sibling, Friend"
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    ID Type <span className="text-red-500">*</span>
                  </label>
                  <select
                    name="guardianIdType"
                    value={formData.guardianIdType}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  >
                    <option value="">Select ID Type</option>
                    <option value="aadhar">Aadhar Card</option>
                    <option value="pan">PAN Card</option>
                    <option value="passport">Passport</option>
                    <option value="driving">Driving License</option>
                  </select>
                </div>
                
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">
                    ID Number <span className="text-red-500">*</span>
                  </label>
                  <input
                    type="text"
                    name="guardianIdNumber"
                    value={formData.guardianIdNumber}
                    onChange={handleChange}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  />
                </div>
              </div>
            </section>

            <section className="mb-8">
              <div className="flex items-center justify-between mb-4">
                <h2 className="text-xl font-semibold text-gray-800">Terms and Conditions</h2>
                <button
                  onClick={() => setShowTerms(!showTerms)}
                  className="text-blue-600 text-sm hover:underline"
                >
                  {showTerms ? 'Hide' : 'Show'} Full Terms
                </button>
              </div>
              
              {showTerms && (
                <div className="bg-gray-50 border border-gray-200 rounded-lg p-6 mb-6 max-h-96 overflow-y-auto text-sm">
                  <h3 className="font-semibold mb-2">Security Deposit and Rent</h3>
                  <p className="mb-4">Security deposit equal to one month's rent must be paid at start. The deposit is non-refundable and will be adjusted against the last month's rent.</p>
                  
                  <h3 className="font-semibold mb-2">House Rules</h3>
                  <ul className="list-disc list-inside mb-4 space-y-1">
                    <li>No smoking inside premises</li>
                    <li>No pets allowed</li>
                    <li>Quiet hours: 9:00 PM to 6:00 AM</li>
                    <li>Maintain cleanliness</li>
                  </ul>
                  
                  <h3 className="font-semibold mb-2">Occupancy Rules</h3>
                  <p className="mb-4">Cannot sublet or allow others to stay without permission. Only tenant and listed local guardian have access.</p>
                  
                  <h3 className="font-semibold mb-2">Termination</h3>
                  <p className="mb-4">Either party can terminate with 1 month notice. Violation of terms may result in immediate eviction.</p>
                  
                  <h3 className="font-semibold mb-2">Liability</h3>
                  <p>Nokreciana Arx Sideralis is not responsible for personal injury, loss, or damage to belongings.</p>
                </div>
              )}
              
              <div className="space-y-4">
                <label className="flex items-start gap-3 cursor-pointer">
                  <input
                    type="checkbox"
                    name="agreeTerms"
                    checked={formData.agreeTerms}
                    onChange={handleChange}
                    className="mt-1 w-5 h-5 text-blue-600 rounded focus:ring-2 focus:ring-blue-500"
                  />
                  <span className="text-sm text-gray-700">
                    I have read and agree to all the terms and conditions of the tenancy agreement
                  </span>
                </label>
                
                <label className="flex items-start gap-3 cursor-pointer">
                  <input
                    type="checkbox"
                    name="agreeDeposit"
                    checked={formData.agreeDeposit}
                    onChange={handleChange}
                    className="mt-1 w-5 h-5 text-blue-600 rounded focus:ring-2 focus:ring-blue-500"
                  />
                  <span className="text-sm text-gray-700">
                    I understand that the security deposit of ₹{rentAmount} is non-refundable and will be adjusted against the last month's rent
                  </span>
                </label>
                
                <label className="flex items-start gap-3 cursor-pointer">
                  <input
                    type="checkbox"
                    name="agreeRules"
                    checked={formData.agreeRules}
                    onChange={handleChange}
                    className="mt-1 w-5 h-5 text-blue-600 rounded focus:ring-2 focus:ring-blue-500"
                  />
                  <span className="text-sm text-gray-700">
                    I agree to follow all house rules including no smoking, no pets, and quiet hours (9 PM - 6 AM)
                  </span>
                </label>
                
                <label className="flex items-start gap-3 cursor-pointer">
                  <i
